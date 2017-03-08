## Типи даних

Кожне значення в Rust має певний *тип*, який каже Rust, дані якого виду 
визначаються, щоб компілятор знав, як працювати з цими даними. У цьому 
підрозділі ми розберемо ряд типів, вбудованих у мову. Ми поділимо типи на дві
категорії: скалярні і складені.

У цьому підрозділі майте на увазі, що Rust - *статично типізована* мова, тобто
тип всіх змінних має бути відомим під час компіляції. Компілятор зазвичай може
вивести, який тип ми хочемо використати, виходячи зі значення і того, як ми його
використовуємо. У випадках, коли можливл багато типів, наприклад якщо ми 
перетворюємо стрічку `String` на число за допомогою `parse` у Розділі 2, треба
додавати позначку типу:

```rust
let guess: u32 = "42".parse().expect("Not a number!");
```

Якщо ми не додамо позначку типу, Rust покаже помилку, яка означає, що 
компілятору треба більше інфомрації від нас, щоб зрозуміти, який з можливих 
типів ми хочемо використати:

```text
error[E0282]: unable to infer enough type information about `_`
 --> src/main.rs:2:5
  |
2 | let guess = "42".parse().expect("Not a number!");
  |     ^^^^^ cannot infer type for `_`
  |
  = note: type annotations or generic parameter binding required
```

Під час подальшого обговорення різних типів даних ви побачите різні позначки 
типів.

### Скалярні типи

*Скалярний* тип представляє єдине значення. У Rust є чотири первинні скалярні 
типи: цілі, числа з рухомою комою, булівські та символи. Ви, швидше за все, 
знаєте їх з інших мов програмування, але давайте поглянемо детальніше на їхню
роботу в Rust.
 
#### Цілі типи

*Ціле* (*integer*) - це число без дробової частини. Ви використали один цілий 
тип раніше в цьому розділі, а саме `i32`. Оголошення цього типу означає, що 
асоційонване з ним значення має бути знаковим цілим (це і позначається `i` від 
англ. integer, на відміну від беззнакового `u` від англ. unsigned) для з 32
двійковими розрядами. Таблиця 3-1 показує вбудовані цілі типи в Rust. Кожен 
варіант в колонках "Знаковий" і "Беззнаковий" (наприклад, *i32*) може використовуватися для проголошення значення цілого типу.

<figure>
<figcaption>

Таблиця 3-1: Цілі типи в Rust

</figcaption>

| Довжина | Знаковий | Беззнаковий |
|---------|----------|-------------|
| 8 біт   | i8       | u8          |
| 16 біт  | i16      | u16         |
| 32 біти | i32      | u32         |
| 64 біти | i64      | u64         |
| архіт.  | isize    | usize       |

</figure>

Кожен варіант може бути знаковим чи беззнаковим і має явно зазначений розмір.
"Знаковий" і "беззнаковий" стосується того, чи може число бути від'ємним чи лише
додатним; іншими словами, чи має число знак (знакове) чи воно буде лише додатним
і, відтак, буде представлене без знаку (беззнакове). Це як запис чисел на 
папері: якщо знак має значення, число записується зі знаком плюс чи знаком 
мінус; але, якщо можна вважати, що число буде додатним, воно записується без 
знаку. Знакові числа зберігаються у [доповняльному коді][compliment].

[compliment]: https://uk.wikipedia.org/wiki/Доповняльний_код

Кожен знаковий варіант може зберігати числа від -(2<sup>n - 1</sup>) до 2<sup>n -
1</sup> - 1 включно, де `n` - кількість біт, які цей варіант використовує. Так,
`i8` може зберігати числа від -(2<sup>7</sup>) до 2<sup>7</sup> - 1, тобто від 
-128 до 127. Беззнакові варіанти зберігають числа від 0 до 2<sup>n</sup> - 1, 
так, `u8` може зберігати числа від 0 до 2<sup>8</sup> - 1, тобто від 0 до 255.

На додачу, типи `isize` та `usize` залежать від різновиду комп'ютера, на якому 
працює ваша програма: 64 біти, якщо це 64-бітна архітектура, чи 32 біти, якщо
32-бітна.

Ви можете писати цілі літерали в будь-якій формі, вказаній у Таблиці 3-2. 
Зверніть увагу, що всі числові літерали, крім байтових літералів, дозволяють
використовувати суфікс типу на кшталт `57u8`, і `_` як роздільник для поліпшення 
читання, як-от `1_000` (те саме, що й `1000`).

<figure>
<figcaption>

Таблиця 3-2: Цілі літерали в Rust

</figcaption>

| Числові літерали   | Приклад       |
|--------------------|---------------|
| Десятковий         | `98_222`      |
| Шістнадцятковий    | `0xff`        |
| Вісімковий         | `0o77`        |
| Двійковий          | `0b1111_0000` |
| Байт (тільки `u8`) | `b'A'`        |

</figure>

Як же здогадатися, який тип цілого використати? Якщо ви непевні, типовий вібір
Rust зазвичай непоганий, а типовий цилий тип в Rust - `i32`: він зазвичай
найшвидший, навіть на 64-бітних системах. Основна ситуація, в якій варто
використовувати `isize` та `usize` - індексація якого виду колекції.

#### Числа з рухомою комою

Також Rust має два первинні типи для *чисел з рухомою комою*, тобто чисел з 
десятковою комою. Числа з рухомою комою в Rust - це `f32` та `f64`, які мають
розмір у 32 біти та 64 біти відповідно. Типовий тип - `f64`, оскільки його 
швидкість приблизно така ж сама, як і в `f32`, але він має вищу точність. На
32-бітних системах можна використовувати тип `f64`, але він буде повільнішим за
`f32` на цих системах. У більшості випадків, вища точність краща за потенційно 
гіршу продуктивність, і варто провести оцінку часу виконання коду (англ. 
benchmark), якщо ви підозрюєте, що розмір чисел з рухомою комою створює проблему
у вашій ситуації.

Ось приклад, що демонструє числа з рухомою комою у дії:

<span class="filename">Файл: src/main.rs</span>

```rust
fn main() {
    let x = 2.0; // f64

    let y: f32 = 3.0; // f32
}
```

Числа з рухомою комою представлені у відповідності зі страндартом IEEE-754. Тип
`f32` є числом одинарної точності, а `f64` має подвійну точність.

#### Числові операції

Rust підтримує звичайні математичні операції, які ви очікуєте для будь-яких 
типів чисел: додавання, віднімання, множення, ділення і остача. Наступний код
демонструє, як використовувати їх у операторії `let`:

<span class="filename">Файл: src/main.rs</span>

```rust
fn main() {
    // додавання
    let sum = 5 + 10;

    // віднімання
    let difference = 95.5 - 4.3;

    // множення
    let product = 4 * 30;

    // ділення
    let quotient = 56.7 / 32.2;

    // остача
    let remainder = 43 % 5;
}
```

Кожен вираз використовує математичний оператор і обчислює значення, яке 
прив'язується до змінної. Додаток B містить список усіх операторів, наданих 
мовою Rust.

#### Булівський тип

Як і в більшості інших мов програмування, булівський тип у Rust має два можливі
значення: `true` ("істина") та `false` ("неправда"). Булівський тип у Rust 
позначається `bool`:

<span class="filename">Файл: src/main.rs</span>

```rust
fn main() {
    let t = true;

    let f: bool = false; // із явною позначкою типу
}
```

Основний спосіб використання булівських значень - умовні вирази, такі, як 
оператор `if`. Про ці вирази розповідається в розділі "Управління потоком 
виконання".

#### Символьний тип

Досі ми працювали тільки з числами, але Rust підтримує також літери. Тип `char`
в Rust є найпростішим алфавітним типом, і цей код демонструє один зі способів
його використання:

<span class="filename">Файл: src/main.rs</span>

```rust
fn main() {
   let c = 'z';
   let z = 'ℤ';
   let heart_eyed_cat = '😻';
}
```
Тип `char` в Rust представляє Скалярне значення Юнікоду, тобто може представляти
значно більше, ніж самий лише ASCII. Наголошені літери, 
китайські/японські/корейські ідеографи, емоджі, і пробіли нульової довжини є
коректними значеннями типу `char` в Rust. Скалярні значення Юнікоду варіюються
від `U+0000` до `U+D7FF` і від `U+E000` до `U+10FFFF` включно. Тим не менш, 
"символ" насправді не є концепцією Юнікоду, тому інтуїція стосовно того, що таке
"символ" може не збігатися із `char` в Rust. Ми обговорюємо це питання 
детальніше у підрозділі "Стрічки" в Розділі 8.

### Compound Types

*Compound types* can group multiple values of other types into one type. Rust
has two primitive compound types: tuples and arrays.

#### Grouping Values into Tuples

A tuple is a general way of grouping together some number of other values with
a variety of types into one compound type.

We create a tuple by writing a comma-separated list of values inside
parentheses. Each position in the tuple has a type, and the types of the
different values in the tuple don’t have to be the same. We’ve added optional
type annotations in this example:

<span class="filename">Filename: src/main.rs</span>

```rust
fn main() {
    let tup: (i32, f64, u8) = (500, 6.4, 1);
}
```

The variable `tup` binds to the entire tuple, since a tuple is considered a
single compound element. To get the individual values out of a tuple, we can
use pattern matching to destructure a tuple value, like this:

<span class="filename">Filename: src/main.rs</span>

```rust
fn main() {
    let tup = (500, 6.4, 1);

    let (x, y, z) = tup;

    println!("The value of y is: {}", y);
}
```

This program first creates a tuple and binds it to the variable `tup`. It then
uses a pattern with `let` to take `tup` and turn it into three separate
variables, `x`, `y`, and `z`. This is called *destructuring*, because it breaks
the single tuple into three parts. Finally, the program prints the value of
`y`, which is `6.4`.

In addition to destructuring through pattern matching, we can also access a
tuple element directly by using a period (`.`) followed by the index of the
value we want to access. For example:

<span class="filename">Filename: src/main.rs</span>

```rust
fn main() {
    let x: (i32, f64, u8) = (500, 6.4, 1);

    let five_hundred = x.0;

    let six_point_four = x.1;

    let one = x.2;
}
```

This program creates a tuple, `x`, and then makes new variables for each
element by using their index. As with most programming languages, the first
index in a tuple is 0.

#### Arrays

Another way to have a collection of multiple values is with an *array*. Unlike
a tuple, every element of an array must have the same type. Arrays in Rust are
different than arrays in some other languages because arrays in Rust have a
fixed length: once declared, they cannot grow or shrink in size.

In Rust, the values going into an array are written as a comma-separated list
inside square brackets:

<span class="filename">Filename: src/main.rs</span>

```rust
fn main() {
    let a = [1, 2, 3, 4, 5];
}
```

Arrays are useful when you want your data allocated on the stack rather than
the heap (we will discuss the stack and the heap more in Chapter 4), or when
you want to ensure you always have a fixed number of elements. They aren’t as
flexible as the vector type, though. The vector type is a similar collection
type provided by the standard library that *is* allowed to grow or shrink in
size. If you’re unsure whether to use an array or a vector, you should probably
use a vector: Chapter 8 discusses vectors in more detail.

An example of when you might want to use an array rather than a vector is in a
program that needs to know the names of the months of the year. It’s very
unlikely that such a program will need to add or remove months, so you can use
an array because you know it will always contain 12 items:

```rust
let months = ["January", "February", "March", "April", "May", "June", "July",
              "August", "September", "October", "November", "December"];
```

##### Accessing Array Elements

An array is a single chunk of memory allocated on the stack. We can access
elements of an array using indexing, like this:

<span class="filename">Filename: src/main.rs</span>

```rust
fn main() {
    let a = [1, 2, 3, 4, 5];

    let first = a[0];
    let second = a[1];
}
```

In this example, the variable named `first` will get the value `1`, because
that is the value at index `[0]` in the array. The variable named `second` will
get the value `2` from index `[1]` in the array.

##### Invalid Array Element Access

What happens if we try to access an element of an array that is past the end of
the array? Say we change the example to the following:

<span class="filename">Filename: src/main.rs</span>

```rust,ignore
fn main() {
    let a = [1, 2, 3, 4, 5];

    let element = a[10];

    println!("The value of element is: {}", element);
}
```

Running this code using `cargo run` produces the following result:

```text
$ cargo run
   Compiling arrays v0.1.0 (file:///projects/arrays)
     Running `target/debug/arrays`
thread '<main>' panicked at 'index out of bounds: the len is 5 but the index is
 10', src/main.rs:4
note: Run with `RUST_BACKTRACE=1` for a backtrace.
error: Process didn't exit successfully: `target/debug/arrays` (exit code: 101)
```

The compilation didn’t produce any errors, but the program results in a
*runtime* error and didn’t exit successfully. When you attempt to access an
element using indexing, Rust will check that the index you’ve specified is less
than the array length. If the index is greater than the length, Rust will
*panic*, which is the term Rust uses when a program exits with an error.

This is the first example of Rust’s safety principles in action. In many
low-level languages, this kind of check is not done, and when you provide an
incorrect index, invalid memory can be accessed. Rust protects you against this
kind of error by immediately exiting instead of allowing the memory access and
continuing. Chapter 9 discusses more of Rust’s error handling.
