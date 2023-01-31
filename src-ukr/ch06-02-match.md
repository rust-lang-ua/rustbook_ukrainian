<!-- Old heading. Do not remove or links may break. -->
<a id="the-match-control-flow-operator"></a>

## Конструкція управління `match`

Rust has an extremely powerful control flow construct called `match` that allows you to compare a value against a series of patterns and then execute code based on which pattern matches. Patterns can be made up of literal values, variable names, wildcards, and many other things; [Chapter 18][ch18-00-patterns]<!-- ignore --> covers all the different kinds of patterns and what they do. The power of `match` comes from the expressiveness of the patterns and the fact that the compiler confirms that all possible cases are handled.

Вираз `match` можна уявити собі як сортувальну машину для монет: монети ковзають жолобом з отворами різних розмірів, і кожна монета падає крізь перший отвір, в який вона проходить. Так само значення проходить крізь кожен шаблон в `match`, і на першому шаблоні, якому воно відповідає, значення "провалюється" в пов'язаний блок коду, де може бути використане при його виконанні.

Оскільки ми згадали монети, використаємо їх як приклад використання `match`! We can write a function that takes an unknown US coin and, in a similar way as the counting machine, determines which coin it is and returns its value in cents, as shown in Listing 6-3.

```rust
{{#rustdoc_include ../listings/ch06-enums-and-pattern-matching/listing-06-03/src/main.rs:here}}
```


<span class="caption">Listing 6-3: An enum and a `match` expression that has the variants of the enum as its patterns</span>

Розберімо `match` у функції `value_in_cents`. First we list the `match` keyword followed by an expression, which in this case is the value `coin`. This seems very similar to a conditional expression used with `if`, but there’s a big difference: with `if`, the condition needs to evaluate to a Boolean value, but here it can be any type. The type of `coin` in this example is the `Coin` enum that we defined on the first line.

Далі йдуть рукави `match`. Рукав має дві частини: шаблон і код. Перший рукав має шаблон, що є значенням `Coin::Penny`, після чого оператор `=>` відокремлює шаблон і код, що буде виконано. Код у цьому випадку - просто значення `1`. Кожен рукав відокремлений від наступного комою.

When the `match` expression executes, it compares the resultant value against the pattern of each arm, in order. Якщо шаблон відповідає значенню, виконується пов'язаний із цим шаблоном код. Якщо шаблон не відповідає значенню, виконання передається наступному рукаву, як монетка в сортувальній машині. Рукавів може бути стільки, скільки нам потрібно: у Блоці коду 6-3 `match` має чотири рукави.

The code associated with each arm is an expression, and the resultant value of the expression in the matching arm is the value that gets returned for the entire `match` expression.

Фігурні дужки зазвичай не використовуються, якщо код рукава match невеликий, як у Блоці коду 6-3, де кожен рукав просто повертає значення. Якщо ви хочете виконати багато рядків коду у рукаві match, то маєте скористатися фігурними дужками, кома після яких в такому разі не обов'язкова. Наприклад, наступний код виводитиме “Lucky penny!” кожного разу, коли метод викличуть для `Coin::Penny`, але також поверне останнє значення блоку, тобто `1`:

```rust
{{#rustdoc_include ../listings/ch06-enums-and-pattern-matching/no-listing-08-match-arm-multiple-lines/src/main.rs:here}}
```

### Patterns That Bind to Values

Інша корисна властивість рукавів match полягає в тому, що вони можуть зв'язуватися з частинами значення, що відповідає шаблону. Таким чином ми можемо дістати значення з варіантів енумів.

Наприклад, змінімо один з варіантів енума, щоб він мав дані усередині. З 1999 по 2008 роки Сполучені Штати карбували четвертаки з різними дизайнами для кожного з 50 штатів на одному боці. Інші монети не мають окремих дизайнів для штатів, тому лише четвертаки мають таке додаткове значення. We can add this information to our `enum` by changing the `Quarter` variant to include a `UsState` value stored inside it, which we’ve done in Listing 6-4.

```rust
{{#rustdoc_include ../listings/ch06-enums-and-pattern-matching/listing-06-04/src/main.rs:here}}
```


<span class="caption">Listing 6-4: A `Coin` enum in which the `Quarter` variant also holds a `UsState` value</span>

Уявімо, що наш друг намагається зібрати всі 50 четвертаків різних штатів. While we sort our loose change by coin type, we’ll also call out the name of the state associated with each quarter so that if it’s one our friend doesn’t have, they can add it to their collection.

У виразі match у цьому коді ми додаємо змінну, що зветься `state` до шаблону, що відповідає значенню варіанту `Coin::Quarter`. Коли шаблон `Coin::Quarter` буде відповідним до виразу, змінна `state` зв'яжеться зі значенням штату цього четвертака. Тоді ми можемо використати `state` у коді цього рукава, ось так:

```rust
{{#rustdoc_include ../listings/ch06-enums-and-pattern-matching/no-listing-09-variable-in-pattern/src/main.rs:here}}
```

Якщо ми викличемо `value_in_cents(Coin::Quarter(UsState::Alaska))`, значення `coin` буде `Coin::Quarter(UsState::Alaska)`. Коли ми порівняємо це значення з усіма рукавами, то не підійде жоден, поки ми не дістанемося `Coin::Quarter(state)`. У цьому місці `state` буде зв'язане зі значенням `UsState::Alaska`. Ми зможемо тоді скористатися цим зв'язуванням у виразі `println!`, отримавши таким чином внутрішнє значення штату з енума `Coin` для варіанту `Quarter`.

### Зіставлення з `Option<T>`

In the previous section, we wanted to get the inner `T` value out of the `Some` case when using `Option<T>`; we can also handle `Option<T>` using `match`, as we did with the `Coin` enum! Instead of comparing coins, we’ll compare the variants of `Option<T>`, but the way the `match` expression works remains the same.

Хай, скажімо, ми хочемо написати функцію, що приймає `Option<i32>`, і якщо він містить значення, додає один до цього значення. А якщо там немає значення всередині, функція має повертати значення `None` і не намагатися виконати жодних дій.

This function is very easy to write, thanks to `match`, and will look like Listing 6-5.

```rust
{{#rustdoc_include ../listings/ch06-enums-and-pattern-matching/listing-06-05/src/main.rs:here}}
```


<span class="caption">Listing 6-5: A function that uses a `match` expression on an `Option<i32>`</span>

Розгляньмо детальніше перше виконання `plus_one`. Коли ми викликаємо `plus_one(five)`, змінна `x` у тілі `plus_one` матиме значення `Some(5)`. We then compare that against each match arm:

```rust,ignore
{{#rustdoc_include ../listings/ch06-enums-and-pattern-matching/listing-06-05/src/main.rs:first_arm}}
```

The `Some(5)` value doesn’t match the pattern `None`, so we continue to the next arm:

```rust,ignore
{{#rustdoc_include ../listings/ch06-enums-and-pattern-matching/listing-06-05/src/main.rs:second_arm}}
```

Чи відповідає `Some(5)` шаблону `Some(i)`? It does! Ми маємо той самий варіант. The `i` binds to the value contained in `Some`, so `i` takes the value `5`. The code in the match arm is then executed, so we add 1 to the value of `i` and create a new `Some` value with our total `6` inside.

Тепер розгляньмо другий виклик `plus_one` у Блоці коду 6-5, де `x` дорівнює `None`. We enter the `match` and compare to the first arm:

```rust,ignore
{{#rustdoc_include ../listings/ch06-enums-and-pattern-matching/listing-06-05/src/main.rs:first_arm}}
```

Підходить! Немає значення, до якого треба додавати, і програма зупиняється і повертає значення `None`, що стоїть праворуч від `=>`. Оскільки перший рукав відповідає значенню, решта рукавів не перевіряються.

Комбінування `match` і енумів корисне в багатьох ситуаціях. Ви часто бачитимете цей шаблон у коді Rust: `match` із енумом, зв'язування змінної з даними усередині, і виконання коду відповідно до цього. Це спершу трохи мудровано, але щойно ви звикнете до цього, то бажатимете мати таку конструкцію в усіх мовах. Ця конструкція - незмінний улюбленець користувачів Rust.

### Match вимагає вичерпності

Є ще один бік `match`, що ми маємо обговорити: шаблони рукавів мають покривати всі можливості. Розгляньте таку версію нашої функції `plus_one`, в якій є вада і вона не скомпілюється:

```rust,ignore,does_not_compile
{{#rustdoc_include ../listings/ch06-enums-and-pattern-matching/no-listing-10-non-exhaustive-match/src/main.rs:here}}
```

Ми не обробили варіанту `None`, тому цей код призводить до вади. На щастя, Rust знає, як виявляти такі вади. Якщо ми спробуємо скомпілювати цей код, то отримаємо таке повідомлення про помилку:

```console
{{#include ../listings/ch06-enums-and-pattern-matching/no-listing-10-non-exhaustive-match/output.txt}}
```

Rust knows that we didn’t cover every possible case, and even knows which pattern we forgot! Match в Rust *вичерпні*: ми маємо вичерпати всі можливі ситуації, щоб код був коректним. Особливо у випадку з `Option<T>`, коли Rust, запобігаючи тому, щоб ми забули явно обробити випадок `None`, захищає нас від припущення, що ми маємо значення, коли ми можемо мати null, таким чином припускаючись помилки на мільярд доларів, про яку ми говорили вище.

### Шаблони для всіх випадків і заповнювач `_`

При роботі з енумами нам може знадобитися особлива дія для кількох конкретних значень, а для всіх інших значень - одна дія за замовчуванням. Уявіть, що ми розробляємо гру, де, якщо ви викинули 3 на кубику, ваш гравець не рухається, а отримує нового модного капелюха. Якщо ви викинете 7, ваш гравець втратить модного капелюха. Для всіх інших значень, ваш гравець рухається на цю кількість клітинок на ігровому полі. Ось `match`, що реалізовує цю логіку, де результат кидання кубика жорстко задано замість випадкового значення, і решта логіки представлена функціями без тіл, бо ми насправді реалізуємо їх поза областю видимості цього прикладу:

```rust
{{#rustdoc_include ../listings/ch06-enums-and-pattern-matching/no-listing-15-binding-catchall/src/main.rs:here}}
```

For the first two arms, the patterns are the literal values `3` and `7`. For the last arm that covers every other possible value, the pattern is the variable we’ve chosen to name `other`. The code that runs for the `other` arm uses the variable by passing it to the `move_player` function.

Цей код компілюється, хоча ми не перерахували усі можливі значення, яких може набути `u8`, бо останній шаблон відповідає всім значенням, які не були вказані окремо. Цей шаблон для всіх випадків задовольняє вимозі вичерпності `match`. Зверніть увагу, що шаблон для всіх випадків розміщується останнім, бо шаблони обчислюються послідовно. Якщо ми розмістимо рукав для всіх випадків раніше, решта рукавів ніколи не запустяться, тому Rust попередить нас, якщо ми після нього додамо ще рукави!

Rust також має шаблон, яким можна скористатися, коли нам потрібно обробити всі випадки, але ми не хочемо *використовувати* значення у шаблоні для всіх випадків: `_` є спеціальним шаблоном, що відповідає будь-якому значенню і не зв'язується із цим значенням. Це каже Rust, що ми не збираємося використовувати це значення, і тому він не попереджатиме про невикористану змінну.

Змінімо правила гри: тепер, якщо ви викинете щось, крім 3 чи 7, то маєте кидати кубик знову. Нам більше не потрібне значення для всіх випадків, тож ми можемо змінити наш код і скористатися `_` замість змінної на ім'я `other`:

```rust
{{#rustdoc_include ../listings/ch06-enums-and-pattern-matching/no-listing-16-underscore-catchall/src/main.rs:here}}
```

This example also meets the exhaustiveness requirement because we’re explicitly ignoring all other values in the last arm; we haven’t forgotten anything.

Finally, we’ll change the rules of the game one more time so that nothing else happens on your turn if you roll anything other than a 3 or a 7. Це можна виразити одиничним значенням (тип порожнього кортежу, який ми згадували у підрозділі [“Тип кортеж”][tuples]<!-- ignore --> ) у коді, що знаходиться у рукаві `_`:

```rust
{{#rustdoc_include ../listings/ch06-enums-and-pattern-matching/no-listing-17-underscore-unit/src/main.rs:here}}
```

Here, we’re telling Rust explicitly that we aren’t going to use any other value that doesn’t match a pattern in an earlier arm, and we don’t want to run any code in this case.

Більш детально про шаблони і зіставлення з ними йдеться у [Розділі 18][ch18-00-patterns]<!-- ignore -->. Ну а поки що ми перейдемо до конструкції `if let`, яка може бути корисною в ситуаціях, де вираз `match` буде надто багатослівним.

[tuples]: ch03-02-data-types.html#the-tuple-type
[ch18-00-patterns]: ch18-00-patterns.html
[ch18-00-patterns]: ch18-00-patterns.html
