## Конструкція управління `match`

Rust має вкрай потужну конструкцію управління, що зветься `match`, яка дозволяє
нам порівнювати значення із кількома шаблонами та потім виконати код, виходячи
з того, який шаблон відповідає цьому значенню. Шаблони можуть складатися з 
літеральних значень, імен змінних, байдужих символів і багатьох інших речей; 
Розділ 18 розкриває усі види шаблонів і що вони роблять. Потужність `match`
походить виразності шаблонів, а компілятор перевіряє, щоб усі можливі варіанти
були оброблені.

Вираз `match` можна уявити собі як сортувальну машину для монет: монети 
ковзають жолобом із отворами різних розмірів, і кожна монета падає крізь 
перший отвір, в який вона проходить. Так само значення проходить крізь кожен
шаблон в `match`, і на першому шаблоні, якому воно відповідає, значення 
"провалюється" в пов'язаний блок коду, де може бути використане.

Оскільки ми згадали монети, використаємо їх як приклад використання `match`! Ми
можемо написати функцію, що приймає невідому монету Сполучених Штатів і, так 
само як і лічільна машина, визначає, яка це монета і повертає її значення в
центах, як показано в Роздруку 6-3:

```rust
enum Coin {
    Penny,
    Nickel,
    Dime,
    Quarter,
}

fn value_in_cents(coin: Coin) -> u32 {
    match coin {
        Coin::Penny => 1,
        Coin::Nickel => 5,
        Coin::Dime => 10,
        Coin::Quarter => 25,
    }
}
```

<span class="caption">Роздрук 6-3: Enum і вираз `match` із варіантами enum-а 
як шаблонами.</span>

Розберемо `match` у функції `value_in_cents`. По-перше, ми пишемо ключове слово
`match`, за яким іде вираз, у цьому випадку - значення `coin`. Це дуже схоже на
вираз, що ми використовувати у `if`, але є велика відмінність: у `if` вираз має
повертати булеве значення. Тут він може бути будь-якого типу. Тип `coin` у 
цьому прикладі - enum `Coin`, визначений у Роздруку 6-3.

Далій йдуть рукави `match`. Рукав має дві частини: шаблон і код. Перший рукав
має шаблон, що є значенням `Coin::Penny`, після якого оператор `=>` відокремлює
шаблон і код, що буде виконано. Код, у цьому випадку - просто значення `1`. 
Кожен рукав відокремлений від наступного комою.

Коли виконується вираз `match`, значення по черзі порівнюється із шаблоном 
кожного рукава. Якщо шаблон відповідає значенню, виконується пов'язаний із цим
шаблоном код. Якщо шаблон не відопвідає значенню, виконання передається 
наступному рукаву, як монетка в сортувальній машині. Рукавів може бути стільки,
скільки нам потрібно: у Роздруку 6-3 `match` має чотири рукави.

Код, пов'язаний з коженим рукавом - вираз, що повертає значення, і кінцеве 
значення виразу рукава, що підійшов, стає значенням, що повертається усім 
виразом `match`.

Фігурні дужки зазвичай не використовуються, якщо код рукава match невеликий, 
як у Роздруку 6-3, де кожен рукав просто повертає значення. Якщо ви хочете 
виконати багато рядків коду у рукаві match, можете скористатися фігурними 
дужками. Наприклад, наступний код виводитиме “Щаслива монетка!” кожного разу,
коли метод викличуть для `Coin::Penny`, але також поверне останнє значення 
блоку, тобто `1`:

```rust
# enum Coin {
#    Penny,
#    Nickel,
#    Dime,
#    Quarter,
# }
#
fn value_in_cents(coin: Coin) -> u32 {
    match coin {
        Coin::Penny => {
            println!("Щаслива монетка!");
            1
        },
        Coin::Nickel => 5,
        Coin::Dime => 10,
        Coin::Quarter => 25,
    }
}
```

### Шаблони, що зв'язуються зі значеннями

Інша корисна властивість рукавів match полягає в тому, що вони можуть 
зв'язуватися з частинами значення, що відповідає шаблону. Таким чином ми можемо
дістати значення з варіантів enum-ів.

Наприклад, змінімо один з варіантів enum-а, щоб він мав дані усередині. З 1999 
по 2008 роки Сполучені Штати карбували чвертаки з різними дизайнами для кожного
з 50 штатів на одному боці. Інші монети не мають окремих дизайнів для штатів,
тому лише чвертаки мають таке додаткове значення. Ми можемо додати цю 
інформацію до нащого `enum`-а, змінивши варіант `Quarter`, аби він включав
значення `UsState` у собі, що й зроблено в Роздруку 6-4:

```rust
#[derive(Debug)] // Щоб можна було швидко подивитися, що за штат
enum UsState {
    Alabama,
    Alaska,
    // ... і т.д.
}

enum Coin {
    Penny,
    Nickel,
    Dime,
    Quarter(UsState),
}
```

<span class="caption">Listing 6-4: Enum `Coin`, де варіант `Quarter` має 
всередині значення `UsState`</span>

Уявімо, що наш друг намагається зібрати всі 50 чвертаків різних штатів. 
Сортуючи дріб'язок по типах монет, ми також будемо називати назви штатів, пов'язаних з кожним чвертаком, щоб, якщо такого друг такого не має, він зміг би
додати його до своєї колекції.

У виразі match у цьому коді ми додаємо змінну, що зветься `state` до шаблону, 
що відповідає значенням варіанту `Coin::Quarter`. Коли шаблон `Coin::Quarter` 
буде відповідним до виразу, змінна `state` зв'яжеться зі значенням стану цього 
чвертака. Тоді ми можемо використати `state` у коді цього рукава, ось так:

```rust
# #[derive(Debug)]
# enum UsState {
#    Alabama,
#    Alaska,
# }
#
# enum Coin {
#    Penny,
#    Nickel,
#    Dime,
#    Quarter(UsState),
# }
#
fn value_in_cents(coin: Coin) -> u32 {
    match coin {
        Coin::Penny => 1,
        Coin::Nickel => 5,
        Coin::Dime => 10,
        Coin::Quarter(state) => {
            println!("Чвертак штату {:?}!", state);
            25
        },
    }
}
```

If we were to call `value_in_cents(Coin::Quarter(UsState::Alaska))`, `coin`
would be `Coin::Quarter(UsState::Alaska)`. When we compare that value with each
of the match arms, none of them match until we reach `Coin::Quarter(state)`. At
that point, the binding for `state` will be the value `UsState::Alaska`. We can
then use that binding in the `println!` expression, thus getting the inner
state value out of the `Coin` enum variant for `Quarter`.

### Matching with `Option<T>`

In the previous section we wanted to get the inner `T` value out of the `Some`
case when using `Option<T>`; we can also handle `Option<T>` using `match` as we
did with the `Coin` enum! Instead of comparing coins, we’ll compare the
variants of `Option<T>`, but the way that the `match` expression works remains
the same.

Let’s say we want to write a function that takes an `Option<i32>`, and if
there’s a value inside, adds one to that value. If there isn’t a value inside,
the function should return the `None` value and not attempt to perform any
operations.

This function is very easy to write, thanks to `match`, and will look like
Listing 6-5:

```rust
fn plus_one(x: Option<i32>) -> Option<i32> {
    match x {
        None => None,
        Some(i) => Some(i + 1),
    }
}

let five = Some(5);
let six = plus_one(five);
let none = plus_one(None);
```

<span class="caption">Listing 6-5: A function that uses a `match` expression on
an `Option<i32>`</span>

#### Matching `Some(T)`

Let’s examine the first execution of `plus_one` in more detail. When we call
`plus_one(five)`, the variable `x` in the body of `plus_one` will have the
value `Some(5)`. We then compare that against each match arm.

```rust,ignore
None => None,
```

The `Some(5)` value doesn’t match the pattern `None`, so we continue to the
next arm.

```rust,ignore
Some(i) => Some(i + 1),
```

Does `Some(5)` match `Some(i)`? Well yes it does! We have the same variant.
The `i` binds to the value contained in `Some`, so `i` takes the value `5`. The
code in the match arm is then executed, so we add one to the value of `i` and
create a new `Some` value with our total `6` inside.

#### Matching `None`

Now let’s consider the second call of `plus_one` in Listing 6-5 where `x` is
`None`. We enter the `match` and compare to the first arm.

```rust,ignore
None => None,
```

It matches! There’s no value to add to, so the program stops and returns the
`None` value on the right side of `=>`. Because the first arm matched, no other
arms are compared.

Combining `match` and enums is useful in many situations. You’ll see this
pattern a lot in Rust code: `match` against an enum, bind a variable to the
data inside, and then execute code based on it. It’s a bit tricky at first, but
once you get used to it, you’ll wish you had it in all languages. It’s
consistently a user favorite.

### Matches Are Exhaustive

There’s one other aspect of `match` we need to discuss. Consider this version
of our `plus_one` function:

```rust,ignore
fn plus_one(x: Option<i32>) -> Option<i32> {
    match x {
        Some(i) => Some(i + 1),
    }
}
```

We didn’t handle the `None` case, so this code will cause a bug. Luckily, it’s
a bug Rust knows how to catch. If we try to compile this code, we’ll get this
error:

```text
error[E0004]: non-exhaustive patterns: `None` not covered
 -->
  |
6 |         match x {
  |               ^ pattern `None` not covered
```

Rust knows that we didn’t cover every possible case and even knows which
pattern we forgot! Matches in Rust are *exhaustive*: we must exhaust every last
possibility in order for the code to be valid. Especially in the case of
`Option<T>`, when Rust prevents us from forgetting to explicitly handle the
`None` case, it protects us from assuming that we have a value when we might
have null, thus making the billion dollar mistake discussed earlier.

### The `_` Placeholder

Rust also has a pattern we can use in situations when we don’t want to list all
possible values. For example, a `u8` can have valid values of 0 through 255. If
we only care about the values 1, 3, 5, and 7, we don’t want to have to list out
0, 2, 4, 6, 8, 9 all the way up to 255. Fortunately, we don’t have to: we can
use the special pattern `_` instead:

```rust
let some_u8_value = 0u8;
match some_u8_value {
    1 => println!("one"),
    3 => println!("three"),
    5 => println!("five"),
    7 => println!("seven"),
    _ => (),
}
```

The `_` pattern will match any value. By putting it after our other arms, the
`_` will match all the possible cases that aren’t specified before it. The `()`
is just the unit value, so nothing will happen in the `_` case. As a result, we
can say that we want to do nothing for all the possible values that we don’t
list before the `_` placeholder.

However, the `match` expression can be a bit wordy in a situation in which we
only care about *one* of the cases. For this situation, Rust provides `if let`.
