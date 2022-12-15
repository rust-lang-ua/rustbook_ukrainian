## Синтаксис Шаблонів

In this section, we gather all the syntax valid in patterns and discuss why and when you might want to use each one.

### Зіставлення з Літералами

Як ви бачили у Розділі 6, можна зіставляти шаблони з літералами напряму. Наведемо декілька прикладів в наступному коді:

```rust
{{#rustdoc_include ../listings/ch18-patterns-and-matching/no-listing-01-literals/src/main.rs:here}}
```

Цей код виведе в консолі `one`, оскільки значення в `x` дорівнює 1. Цей синтаксис корисний, коли ви хочете, щоб ваш код виконував дію, якщо він отримує певне значення.

### Зіставлення з Найменованими Змінними

Іменовані змінні - це незаперечні шаблони, які відповідають будь-якому значенню, і ми багато разів використовували їх у книзі. Однак, існує ускладнення при використанні іменованих змінних у виразах `match`. Оскільки `match` починає нову область видимості, змінні, оголошені як частина шаблону всередині виразу `match`, будуть затінювати змінні з тією ж назвою за межами конструкції `match`, як і у випадку з усіма змінними. У Блоці Коду 18-11 оголошується змінна з назвою `x` зі значенням `Some(5)` та змінна `y` зі значенням `10`. Потім ми створюємо вираз `match` над значенням `x`. Подивіться на шаблони в рукавах match і `println!` наприкінці, і перед тим, як запускати цей код або читати далі, спробуйте з'ясувати, що виведе код в консолі.

<span class="filename">Файл: src/main.rs</span>

```rust
{{#rustdoc_include ../listings/ch18-patterns-and-matching/listing-18-11/src/main.rs:here}}
```


<span class="caption">Listing 18-11: A `match` expression with an arm that introduces a shadowed variable `y`</span>

Розглянемо, що відбувається при виконанні виразу `match`. Шаблон у першому рукаві порівняння не збігається із заданим значенням `x`, тому код продовжується.

Шаблон у другому рукаві порівняння вводить нову змінну з назвою `y`, яка буде відповідати будь-якому значенню всередині значення `Some`. Оскільки ми знаходимося в новій області видимості всередині виразу `match`, це нова змінна `y`, а не та `y`, яку ми оголосили на початку зі значенням 10. Ця нова прив'язка `y` буде відповідати будь-якому значенню всередині `Some`, яке ми маємо в `x`. Таким чином, ця нова `y` зв'язується з внутрішнім значенням `Some` в `x`. Це значення `5`, тому вираз для цього рукава виконується і виводить в консолі `Matched, y = 5`.

Якби значення `x` було б `None` замість `Some(5)`, шаблони в перших двох рукавах не збіглися б, тому значення збіглося б з підкресленням. Ми не створювали змінну `x` у шаблоні підкреслення, тому `x` у виразі - це все ще зовнішній `x`, який не був затінений. У цьому гіпотетичному випадку `match` виведе в консолі `Default case, x = None`.

Коли вираз `match` виконано, його область видимості закінчується, так само як і область видимості внутрішньої `y`. Останній `println!` виведе в консолі `at the end: x = Some(5), y = 10`.

Щоб створити вираз `match`, який порівнює значення зовнішніх `x` і `y`, замість того, щоб вводити затінену змінну, нам потрібно буде використовувати умовний запобіжник. Ми поговоримо про запобіжники пізніше в розділі ["Додаткові умови з запобіжниками"](#extra-conditionals-with-match-guards)<!--
ignore --> .

### Декілька Шаблонів

У виразах `match` ви можете зіставляти кілька шаблонів, використовуючи синтаксис `|`, який є оператором шаблону *or*. Наприклад, у наступному коді ми порівнюємо значення `x` з рукавами match, перше з яких має опцію *or*, що означає, що якщо значення `x` збігається з будь-яким зі значень у цьому рукаві, код цього рукава буде виконано:

```rust
{{#rustdoc_include ../listings/ch18-patterns-and-matching/no-listing-02-multiple-patterns/src/main.rs:here}}
```

Цей код виведе в консоль `one or two`.

### Зіставлення Діапазонів Значень з `..=`

Синтаксис `..=` дозволяє робити інклюзивне зіставлення, зіставлення з діапазоном включно з останнім його значенням. В наступному коді буде виконана гілка, шаблон якої зіставляється з будь-яким значенням заданого діапазону:

```rust
{{#rustdoc_include ../listings/ch18-patterns-and-matching/no-listing-03-ranges/src/main.rs:here}}
```

Якщо `x` дорівнює 1, 2, 3, 4, або 5, то буде обрана перша гілка виразу match. Цей синтаксис більш зручний для зіставлення декількох значень ніж використання оператора `|` для вираження тої самої ідеї; якщо ми використовували б `|`, нам було б потрібно вказати `1 | 2 | 3 | 4 | 5`. Вказання діапазону набагато коротше, особливо якщо ми хочемо зіставляти, скажімо, будь-яке число між 1 та 1,000!

The compiler checks that the range isn’t empty at compile time, and because the only types for which Rust can tell if a range is empty or not are `char` and numeric values, ranges are only allowed with numeric or `char` values.

Ось приклад використання діапазонів значень `char`:

```rust
{{#rustdoc_include ../listings/ch18-patterns-and-matching/no-listing-04-ranges-of-char/src/main.rs:here}}
```

Rust може визначити, що `'c'` в першому діапазоні шаблона та виведе в консоль `early ASCII letter`.

### Деструктуризація для Розбору Значень на Частини

Ми також використовуємо шаблони для деструктуризації структур, енумів та кортежів для використання різних частин їх значень. Розглянемо покроково кожне значення.

#### Деструктуризація Структур

Listing 18-12 shows a `Point` struct with two fields, `x` and `y`, that we can break apart using a pattern with a `let` statement.

<span class="filename">Файл: src/main.rs</span>

```rust
{{#rustdoc_include ../listings/ch18-patterns-and-matching/listing-18-12/src/main.rs}}
```


<span class="caption">Listing 18-12: Destructuring a struct’s fields into separate variables</span>

В цьому коді створюються змінні `a` та `b`, які відповідають значенням полів `x` та `y` структури `p`. Цей приклад показує, що назви змінних у шаблоні не обов'язково повинні збігатися з назвами полів структури. Однак, зазвичай назви змінних збігаються з назвами полів, щоб полегшити запам'ятовування того, які змінні походять з яких полів. Через таке поширене використання, а також через те, що запис `let Point { x: x, y: y } = p;` містить багато повторень, Rust має скорочення для шаблонів, які відповідають полям struct: вам потрібно лише перерахувати назву поля struct, і змінні, створені на основі шаблону, матимуть ті ж самі назви. Блок Коду 18-13 працює так само як і Блок Коду 18-12, але змінні, що створюються в шаблоні `let`, є `x` і `y` замість `a` і `b`.

<span class="filename">Файл: src/main.rs</span>

```rust
{{#rustdoc_include ../listings/ch18-patterns-and-matching/listing-18-13/src/main.rs}}
```


<span class="caption">Listing 18-13: Destructuring struct fields using struct field shorthand</span>

Цей код створить змінні `x` та `y`, які відповідають полям `x` та`y` змінної `p`. В результаті змінні `x` та `y` містять значення зі структури `p`.

Ми також можемо деструктурувати за допомогою буквених значень як частини шаблону struct замість того, щоб створювати змінні для всіх полів. Це дозволяє нам перевіряти деякі з полів на наявність певних значень, створюючи змінні для деструктуризації інших полів.

In Listing 18-14, we have a `match` expression that separates `Point` values into three cases: points that lie directly on the `x` axis (which is true when `y = 0`), on the `y` axis (`x = 0`), or neither.

<span class="filename">Файл: src/main.rs</span>

```rust
{{#rustdoc_include ../listings/ch18-patterns-and-matching/listing-18-14/src/main.rs:here}}
```


<span class="caption">Listing 18-14: Destructuring and matching literal values in one pattern</span>

Перший рукав буде відповідати будь-якій точці, що лежить на осі `x`, вказуючи, що поле `y` збігається, якщо його значення збігається з `0`. Шаблон все ще створює змінну `x`, яку ми можемо використовувати в коді для цього рукава.

Аналогічно, другий рукав зіставляє будь-яку точку на осі `y`, вказуючи, що поле `x` збігається, якщо його значення дорівнює `0`, і створює змінну `y` для значення поля `y`. Третій рукав не визначає ніяких літералів, тому воно відповідає будь-якій іншій `Point` і створює змінні для полів `x` і `y`.

In this example, the value `p` matches the second arm by virtue of `x` containing a 0, so this code will print `On the y axis at 7`.

Remember that a `match` expression stops checking arms once it has found the first matching pattern, so even though `Point { x: 0, y: 0}` is on the `x` axis and the `y` axis, this code would only print `On the x axis at 0`.

#### Деструктуризація Енумів

У цій книзі ми вже деструктурували енуми (наприклад, в Блоці Коду 6-5 Розділу 6), але ми ще окремо не обговорювали, що шаблон деструктурування енума повинен відповідати тому, як визначаються збережені в енумі дані. Як приклад, у Блоці Коду 18-15 ми використовуємо енум `Message` з Блоку Коду 6-2 і пишемо `match` з шаблонами, які деструктуруватимуть кожне внутрішнє значення.

<span class="filename">Файл: src/main.rs</span>

```rust
{{#rustdoc_include ../listings/ch18-patterns-and-matching/listing-18-15/src/main.rs}}
```


<span class="caption">Listing 18-15: Destructuring enum variants that hold different kinds of values</span>

Цей код виведе в консолі `Change the color to red 0, green 160, and blue 255`. Спробуйте змінити значення `msg`, щоб побачити виконання коду з інших рукавів.

Для варіантів енуму без даних, таких як `Message::Quit`, ми не можемо деструктурувати значення далі. Ми тільки можемо зіставити буквальне значення `Message::Quit`, і жодних змінних у цьому шаблоні немає.

Для структуро-подібних варіантів енуму, таких як `Message::Move`, ми можемо використовувати шаблон схожий з тим, що ми вказували для зіставлення структур. Після назви варіанту ми ставимо фігурні дужки, а потім перелічуємо поля зі змінними, щоб розбити все на частини, які будуть використані в коді для цього рукава. Тут ми використовуємо скорочену форму, як ми це робили в Блоці Коду 18-13.

Шаблони кортежо-подібних варіантів енума, таких як `Message::Write`, що містить кортеж з одним елементом, і `Message::ChangeColor`, що містить кортеж з трьома елементами подібні до шаблону, який ми вказуємо для зіставлення кортежів. Кількість змінних у шаблоні повинна відповідати кількості елементів у варіанті, який ми порівнюємо.

#### Деструктуризація Вкладених Структур та Енумів

Дотепер всі наші приклади стосувалися зіставлення структур або енумів глибиною в один рівень, але зіставлення може працювати і на вкладених елементах! Наприклад, ми можемо переробити код у Блоці Коду 18-15 для додавання підтримки RGB та HSV кольорів у повідомленні `ChangeColor`, як показано у Блоці Коду 18-16.

```rust
{{#rustdoc_include ../listings/ch18-patterns-and-matching/listing-18-16/src/main.rs}}
```

<span class="caption">Блок Коду 18-16: Зіставлення з вкладеними енумами</span>

Шаблон першого рукава у виразі `match` відповідає варіанту енуму `Message::ChangeColor`, який містить варіант `Color::Rgb`; потім шаблон зв'язується з трьома внутрішніми значеннями `i32`. Шаблон другого рукава також відповідає варіанту енуму `Message::ChangeColor`, але внутрішній енум замість цього збігається з `Color::Hsv`. Ми можемо вказувати такі складні умови в одному виразі `match`, навіть якщо залучені два енуми.

#### Деструктуризація Структур та Кортежів

Ми можемо змішувати, зіставляти та вкладати деструктуризуючі шаблони і складнішими способами. В наступному прикладі показано складна деструктуризація, де ми вкладаємо структури та кортежі в кортеж та деструктуризуємо все примітивні значення:

```rust
{{#rustdoc_include ../listings/ch18-patterns-and-matching/no-listing-05-destructuring-structs-and-tuples/src/main.rs:here}}
```

This code lets us break complex types into their component parts so we can use the values we’re interested in separately.

Destructuring with patterns is a convenient way to use pieces of values, such as the value from each field in a struct, separately from each other.

### Ігнорування Значень Шаблона

You’ve seen that it’s sometimes useful to ignore values in a pattern, such as in the last arm of a `match`, to get a catchall that doesn’t actually do anything but does account for all remaining possible values. There are a few ways to ignore entire values or parts of values in a pattern: using the `_` pattern (which you’ve seen), using the `_` pattern within another pattern, using a name that starts with an underscore, or using `..` to ignore remaining parts of a value. Розглянемо, як і навіщо використовувати кожен з цих шаблонів.

#### Ігнорування цілого значення з _ `_`

Ми використали символ підкреслення як шаблон підстановки, який буде відповідати будь-якому значенню, але не прив'язуватиметься до нього. This is especially useful as the last arm in a `match` expression, but we can also use it in any pattern, including function parameters, as shown in Listing 18-17.

<span class="filename">Файл: src/main.rs</span>

```rust
{{#rustdoc_include ../listings/ch18-patterns-and-matching/listing-18-17/src/main.rs}}
```

<span class="caption">Блок Коду 18-17: Використання `_` в сигнатурі функції</span>

This code will completely ignore the value `3` passed as the first argument, and will print `This code only uses the y parameter: 4`.

In most cases when you no longer need a particular function parameter, you would change the signature so it doesn’t include the unused parameter. Ignoring a function parameter can be especially useful in cases when, for example, you're implementing a trait when you need a certain type signature but the function body in your implementation doesn’t need one of the parameters. You then avoid getting a compiler warning about unused function parameters, as you would if you used a name instead.

#### Ignoring Parts of a Value with a Nested `_`

We can also use `_` inside another pattern to ignore just part of a value, for example, when we want to test for only part of a value but have no use for the other parts in the corresponding code we want to run. Listing 18-18 shows code responsible for managing a setting’s value. The business requirements are that the user should not be allowed to overwrite an existing customization of a setting but can unset the setting and give it a value if it is currently unset.

```rust
{{#rustdoc_include ../listings/ch18-patterns-and-matching/listing-18-18/src/main.rs:here}}
```


<span class="caption">Listing 18-18: Using an underscore within patterns that match `Some` variants when we don’t need to use the value inside the `Some`</span>

This code will print `Can't overwrite an existing customized value` and then `setting is Some(5)`. In the first match arm, we don’t need to match on or use the values inside either `Some` variant, but we do need to test for the case when `setting_value` and `new_setting_value` are the `Some` variant. In that case, we print the reason for not changing `setting_value`, and it doesn’t get changed.

In all other cases (if either `setting_value` or `new_setting_value` are `None`) expressed by the `_` pattern in the second arm, we want to allow `new_setting_value` to become `setting_value`.

We can also use underscores in multiple places within one pattern to ignore particular values. Listing 18-19 shows an example of ignoring the second and fourth values in a tuple of five items.

```rust
{{#rustdoc_include ../listings/ch18-patterns-and-matching/listing-18-19/src/main.rs:here}}
```

<span class="caption">Блок Коду 18-19: Ігнорування кількох частин кортежу</span>

This code will print `Some numbers: 2, 8, 32`, and the values 4 and 16 will be ignored.

#### Ignoring an Unused Variable by Starting Its Name with `_`

If you create a variable but don’t use it anywhere, Rust will usually issue a warning because an unused variable could be a bug. However, sometimes it’s useful to be able to create a variable you won’t use yet, such as when you’re prototyping or just starting a project. In this situation, you can tell Rust not to warn you about the unused variable by starting the name of the variable with an underscore. In Listing 18-20, we create two unused variables, but when we compile this code, we should only get a warning about one of them.

<span class="filename">Файл: src/main.rs</span>

```rust
{{#rustdoc_include ../listings/ch18-patterns-and-matching/listing-18-20/src/main.rs}}
```


<span class="caption">Listing 18-20: Starting a variable name with an underscore to avoid getting unused variable warnings</span>

Here we get a warning about not using the variable `y`, but we don’t get a warning about not using `_x`.

Note that there is a subtle difference between using only `_` and using a name that starts with an underscore. The syntax `_x` still binds the value to the variable, whereas `_` doesn’t bind at all. To show a case where this distinction matters, Listing 18-21 will provide us with an error.

```rust,ignore,does_not_compile
{{#rustdoc_include ../listings/ch18-patterns-and-matching/listing-18-21/src/main.rs:here}}
```


<span class="caption">Listing 18-21: An unused variable starting with an underscore still binds the value, which might take ownership of the value</span>

We’ll receive an error because the `s` value will still be moved into `_s`, which prevents us from using `s` again. However, using the underscore by itself doesn’t ever bind to the value. Listing 18-22 will compile without any errors because `s` doesn’t get moved into `_`.

```rust
{{#rustdoc_include ../listings/ch18-patterns-and-matching/listing-18-22/src/main.rs:here}}
```


<span class="caption">Listing 18-22: Using an underscore does not bind the value</span>

Цей код працює, оскільки ми ніколи та ні до чого не прив'язували `s`; воно не зміщене.

#### Ігнорування Інших Частин Значення з `..`

With values that have many parts, we can use the `..` syntax to use specific parts and ignore the rest, avoiding the need to list underscores for each ignored value. The `..` pattern ignores any parts of a value that we haven’t explicitly matched in the rest of the pattern. In Listing 18-23, we have a `Point` struct that holds a coordinate in three-dimensional space. In the `match` expression, we want to operate only on the `x` coordinate and ignore the values in the `y` and `z` fields.

```rust
{{#rustdoc_include ../listings/ch18-patterns-and-matching/listing-18-23/src/main.rs:here}}
```


<span class="caption">Listing 18-23: Ignoring all fields of a `Point` except for `x` by using `..`</span>

We list the `x` value and then just include the `..` pattern. This is quicker than having to list `y: _` and `z: _`, particularly when we’re working with structs that have lots of fields in situations where only one or two fields are relevant.

The syntax `..` will expand to as many values as it needs to be. Listing 18-24 shows how to use `..` with a tuple.

<span class="filename">Файл: src/main.rs</span>

```rust
{{#rustdoc_include ../listings/ch18-patterns-and-matching/listing-18-24/src/main.rs}}
```


<span class="caption">Listing 18-24: Matching only the first and last values in a tuple and ignoring all other values</span>

In this code, the first and last value are matched with `first` and `last`. `..` буде зіставлятися та ігнорувати зі всім посередині.

However, using `..` must be unambiguous. If it is unclear which values are intended for matching and which should be ignored, Rust will give us an error. Listing 18-25 shows an example of using `..` ambiguously, so it will not compile.

<span class="filename">Файл: src/main.rs</span>

```rust,ignore,does_not_compile
{{#rustdoc_include ../listings/ch18-patterns-and-matching/listing-18-25/src/main.rs}}
```


<span class="caption">Listing 18-25: An attempt to use `..` in an ambiguous way</span>

Якщо ми скомпілюємо цей приклад, ми отримаємо цю помилку:

```console
{{#include ../listings/ch18-patterns-and-matching/listing-18-25/output.txt}}
```

It’s impossible for Rust to determine how many values in the tuple to ignore before matching a value with `second` and then how many further values to ignore thereafter. This code could mean that we want to ignore `2`, bind `second` to `4`, and then ignore `8`, `16`, and `32`; or that we want to ignore `2` and `4`, bind `second` to `8`, and then ignore `16` and `32`; and so forth. The variable name `second` doesn’t mean anything special to Rust, so we get a compiler error because using `..` in two places like this is ambiguous.

### Додаткові Умови з Запобіжниками Зіставлення

A *match guard* is an additional `if` condition, specified after the pattern in a `match` arm, that must also match for that arm to be chosen. Match guards are useful for expressing more complex ideas than a pattern alone allows.

The condition can use variables created in the pattern. Listing 18-26 shows a `match` where the first arm has the pattern `Some(x)` and also has a match guard of `if x % 2 == 0` (which will be true if the number is even).

```rust
{{#rustdoc_include ../listings/ch18-patterns-and-matching/listing-18-26/src/main.rs:here}}
```

<span class="caption">Блок Коду 18-26: Додавання запобіжника зіставлення до шаблона</span>

Цей приклад виведе в консолі `The number 4 is even`. When `num` is compared to the pattern in the first arm, it matches, because `Some(4)` matches `Some(x)`. Then the match guard checks whether the remainder of dividing `x` by 2 is equal to 0, and because it is, the first arm is selected.

If `num` had been `Some(5)` instead, the match guard in the first arm would have been false because the remainder of 5 divided by 2 is 1, which is not equal to 0. Rust would then go to the second arm, which would match because the second arm doesn’t have a match guard and therefore matches any `Some` variant.

There is no way to express the `if x % 2 == 0` condition within a pattern, so the match guard gives us the ability to express this logic. The downside of this additional expressiveness is that the compiler doesn't try to check for exhaustiveness when match guard expressions are involved.

In Listing 18-11, we mentioned that we could use match guards to solve our pattern-shadowing problem. Recall that we created a new variable inside the pattern in the `match` expression instead of using the variable outside the `match`. That new variable meant we couldn’t test against the value of the outer variable. Listing 18-27 shows how we can use a match guard to fix this problem.

<span class="filename">Файл: src/main.rs</span>

```rust
{{#rustdoc_include ../listings/ch18-patterns-and-matching/listing-18-27/src/main.rs}}
```


<span class="caption">Listing 18-27: Using a match guard to test for equality with an outer variable</span>

Цей код виведе в консолі `Default case, x = Some(5)`. The pattern in the second match arm doesn’t introduce a new variable `y` that would shadow the outer `y`, meaning we can use the outer `y` in the match guard. Instead of specifying the pattern as `Some(y)`, which would have shadowed the outer `y`, we specify `Some(n)`. This creates a new variable `n` that doesn’t shadow anything because there is no `n` variable outside the `match`.

Запобіжник зіставлення `if n == y` не є шаблоном і тому не вводить нових змінних. This `y` *is* the outer `y` rather than a new shadowed `y`, and we can look for a value that has the same value as the outer `y` by comparing `n` to `y`.

You can also use the *or* operator `|` in a match guard to specify multiple patterns; the match guard condition will apply to all the patterns. Listing 18-28 shows the precedence when combining a pattern that uses `|` with a match guard. The important part of this example is that the `if y` match guard applies to `4`, `5`, *and* `6`, even though it might look like `if y` only applies to `6`.

```rust
{{#rustdoc_include ../listings/ch18-patterns-and-matching/listing-18-28/src/main.rs:here}}
```


<span class="caption">Listing 18-28: Combining multiple patterns with a match guard</span>

The match condition states that the arm only matches if the value of `x` is equal to `4`, `5`, or `6` *and* if `y` is `true`. When this code runs, the pattern of the first arm matches because `x` is `4`, but the match guard `if y` is false, so the first arm is not chosen. The code moves on to the second arm, which does match, and this program prints `no`. The reason is that the `if` condition applies to the whole pattern `4 | 5 | 6`, not only to the last value `6`. In other words, the precedence of a match guard in relation to a pattern behaves like this:

```text
(4 | 5 | 6) if y => ...
```

замість:

```text
4 | 5 | (6 if y) => ...
```

After running the code, the precedence behavior is evident: if the match guard were applied only to the final value in the list of values specified using the `|` operator, the arm would have matched and the program would have printed `yes`.

### `@` Bindings

The *at* operator `@` lets us create a variable that holds a value at the same time as we’re testing that value for a pattern match. In Listing 18-29, we want to test that a `Message::Hello` `id` field is within the range `3..=7`. We also want to bind the value to the variable `id_variable` so we can use it in the code associated with the arm. We could name this variable `id`, the same as the field, but for this example we’ll use a different name.

```rust
{{#rustdoc_include ../listings/ch18-patterns-and-matching/listing-18-29/src/main.rs:here}}
```


<span class="caption">Listing 18-29: Using `@` to bind to a value in a pattern while also testing it</span>

Цей приклад виведе в консолі `Found an id in range: 5`. By specifying `id_variable
@` before the range `3..=7`, we’re capturing whatever value matched the range while also testing that the value matched the range pattern.

In the second arm, where we only have a range specified in the pattern, the code associated with the arm doesn’t have a variable that contains the actual value of the `id` field. The `id` field’s value could have been 10, 11, or 12, but the code that goes with that pattern doesn’t know which it is. The pattern code isn’t able to use the value from the `id` field, because we haven’t saved the `id` value in a variable.

In the last arm, where we’ve specified a variable without a range, we do have the value available to use in the arm’s code in a variable named `id`. The reason is that we’ve used the struct field shorthand syntax. But we haven’t applied any test to the value in the `id` field in this arm, as we did with the first two arms: any value would match this pattern.

Using `@` lets us test a value and save it in a variable within one pattern.

## Підсумок

Шаблони в Rust дуже корисні для визначення різниці між різновидами даних. When used in `match` expressions, Rust ensures your patterns cover every possible value, or your program won’t compile. Patterns in `let` statements and function parameters make those constructs more useful, enabling the destructuring of values into smaller parts at the same time as assigning to variables. We can create simple or complex patterns to suit our needs.

Next, for the penultimate chapter of the book, we’ll look at some advanced aspects of a variety of Rust’s features.
