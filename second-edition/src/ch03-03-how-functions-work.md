## Як працюють функції.

Функції використовуються скрізь у коді на Rust. Ви вже бачили одну з 
найважливіших функцій у мові - функцію `main`, яка є точкою входу багатьох 
програм. Ви також бачили ключове слово `fn`, яке дозволяє вам оголошувати нові 
функції.

У мові Rust для назв функцій і змінних прийнято використовувати *зміїний 
регістр* - тобто всі літери маленькі, а слова відокремлюються підкресленянми.
Ось приклад програми, що містить визначення функції:

<span class="filename">Файл: src/main.rs</span>

```rust
fn main() {
    println!("Привіт, світ!");

    another_function();
}

fn another_function() {
    println!("Інша функція.");
}
```

Визначення функцій у Rust починаються з `fn` і мають кілька пар дужок після 
назви функції. Фігурні дужки кажуть компілятору, де починається і закінчується 
тіло функції.

Ми можемо викликати будь-яку визначену нами функцію, написавши її назву і пару
дужок. Оскільки `another_function` визначена в програмі, її можна викликати
зсередини функції `main`. Зверніть увагу, що ми визначили `another_function` 
у сирцевому коді *після* функції `main`; так само її можна було визначити до
функції `main`. Для Rust не має значення, де ви визначаєте функції, важливо,
щоб вони були визначені хоч десь.

Почнемо новий бінарний проект з назвою *functions*, щоб глибше дослідити 
функції. Помістіть приклад `another_function` до файлу *src/main.rs* і запустіть
його. Ви побачите таке:

```text
$ cargo run
   Compiling functions v0.1.0 (file:///projects/functions)
     Running `target/debug/functions`
Привіт, світ!
Інша функція.
```

Рядки виконуються в порядку, в якому вони знаходяться в функції `main`. Спершу
виводиться повідомлення “Привіт, світ!”, а потім викликається `another_function`
і виводить своє повідомлення.

### Параметри функції

При визначення функціям можна визначати *параметри* - особливі змінні, що є 
частиною визначення функції. Коли функція має параметри, ми можемо надати 
функції конкретні значення для цих параметрів. Формально, конкретні значення 
звуться *аргументами* або *фактичними параметрами*, а параметри у визначенні
функції - *формальними параметрами*, але зазвичай слова "параметр" та "аргумент"
використовуються як для частини визначення функції, так і для конкретних 
значень, які були передані при виклику функції.

Це переписана версія `another_function` демонструє, як виглядають параметри в 
Rust:

<span class="filename">Файл: src/main.rs</span>

```rust
fn main() {
    another_function(5);
}

fn another_function(x: i32) {
    println!("Значення x: {}", x);
}
```

Запустіть цю програму; ви маєте побачити таке:

```text
$ cargo run
   Compiling functions v0.1.0 (file:///projects/functions)
     Running `target/debug/functions`
Значення x: 5
```

Проголошення `another_function` містить один параметр під назвою `x`. Тип `x` 
визначено як `i32`. Коли в `another_function` передається `5`, макрос `println!`
виведе `5` на місце фігурних дужок у рядку формату.

У проголошенні функції ви *обов'язково* маєте проголошувати тип кожного 
параметру. Це свідоме рішення у дизайні мови Rust: вимога позначати тип у 
визначенні функції означає, що компілятору дуже рідко знадобиться просити вас
використовувати їх де-інде ще в коді, щоб зрозуміти, який тип вам потрібен.

Якщо ви хочете, щоб у функції було багато параметрів, відокремлюйте проголошення
параметрів комами, ось так:

<span class="filename">Файл: src/main.rs</span>

```rust
fn main() {
    another_function(5, 6);
}

fn another_function(x: i32, y: i32) {
    println!("Значення x: {}", x);
    println!("Значення y: {}", y);
}
```

Цей приклад створює функцію з двома параметрами, обидва мають тип `i32`. Функція
виводить значення обох своїх параметрів. Звісно, параметрам функції зовсім не 
обов'язково мати один тип - просто так зроблено в цьому прикладі.

Спробуймо запустити цей код. Замініть програму у файлі *src/main.rs* вашого
проекту *function* кодом вище, і запустіть його командою `cargo run`:

```text
$ cargo run
   Compiling functions v0.1.0 (file:///projects/functions)
     Running `target/debug/functions`
Значення x: 5
Значення y: 6
```

Оскільки ми викликали функцію зі значенням `5` для параметру `x` і значенням `6` 
для `y`, обидві стрічки виведені зі цими значеннями. 

### Тіла функцій

Тіла функцій складаються з серії операторів, яка може закінчуватися виразом. 
Поки що ми описували тільки функції без виразу наприкінці, але використовували
вирази як частини операторів. Оскільки Rust є мовою, базованою на виразах, 
важливо розуміти цю відмінність. Інші мови можуть не мати таких відмінностей, 
тому давайте розглянемо, що таке оператори і вирази і як різниця між ними 
впливає на тіла функцій.

### Оператори і вирази

Насправді ми вже використовували оператори і вирази. *Оператори* - це 
інструкції, що виконують певні дії і не повертають значення. *Вирази* 
обчислюються, в результаті даючи певне значення. Поглянемо на приклади.

Створення змінної і надання їй значення за допомогою ключового слова `let` - це
оператор. У Роздруку 3-3 `let y = 6;` є оператором:

<figure>
<span class="filename">Файл: src/main.rs</span>

```rust
fn main() {
    let y = 6;
}
```

<figcaption>

Listing 3-3: Проголошення функції `main`, що містить один оператор.

</figcaption>
</figure>

Проголошення функцій - також оператори; весь попередній приклад є одним складним 
оператором.

Оператори не повертають значень. Таким чином, не можна присвоїти оператор `let`
іншій змінній, на кшталт такого:

<span class="filename">Файл: src/main.rs</span>

```rust,ignore
fn main() {
    let x = (let y = 6);
}
```

При спробі запустити цю програму, ви отримаєте повідомлення про помилку:

```text
$ cargo run
   Compiling functions v0.1.0 (file:///projects/functions)
error: expected expression, found statement (`let`)
 --> src/main.rs:2:14
  |
2 |     let x = (let y = 6);
  |              ^^^
  |
  = note: variable declaration using `let` is a statement
```

Оператор `let y = 6` не повертає значення, тому немає нічого, з чим можна було б
зв'язати `x`. Це відрізняється від інших мов, таких як C чи Ruby, де присвоєння
повертає значення, яке воно присвоїло. В тих мовах можна написати `x = y = 6` і
обидві змінні `x` та `y` набудуть значення `6`; в Rust так робити не можна.

Вирази
Expressions evaluate to something and make up most of the rest of the code that
you’ll write in Rust. Consider a simple math operation, such as `5 + 6`, which
is an expression that evaluates to the value `11`. Expressions can be part of
statements: in Listing 3-3 that had the statement `let y = 6;`, `6` is an
expression that evaluates to the value `6`. Calling a function is an
expression. Calling a macro is an expression. The block that we use to create
new scopes, `{}`, is an expression, for example:

<span class="filename">Filename: src/main.rs</span>

```rust
fn main() {
    let x = 5;

    let y = {
        let x = 3;
        x + 1
    };

    println!("The value of y is: {}", y);
}
```

This expression:

```rust,ignore
{
    let x = 3;
    x + 1
}
```

is a block that, in this case, evaluates to `4`. That value gets bound to `y`
as part of the `let` statement. Note the line without a semicolon at the end,
unlike most of the lines you’ve seen so far. Expressions do not include ending
semicolons. If you add a semicolon to the end of an expression, you turn it
into a statement, which will then not return a value. Keep this in mind as you
explore function return values and expressions next.

### Functions with Return Values

Functions can return values to the code that calls them. We don’t name return
values, but we do declare their type after an arrow (`->`). In Rust, the return
value of the function is synonymous with the value of the final expression in
the block of the body of a function. Here’s an example of a function that
returns a value:

<span class="filename">Filename: src/main.rs</span>

```rust
fn five() -> i32 {
    5
}

fn main() {
    let x = five();

    println!("Значення x: {}", x);
}
```

There are no function calls, macros, or even `let` statements in the `five`
function—just the number `5` by itself. That’s a perfectly valid function in
Rust. Note that the function’s return type is specified, too, as `-> i32`. Try
running this code; the output should look like this:

```text
$ cargo run
   Compiling functions v0.1.0 (file:///projects/functions)
     Running `target/debug/functions`
Значення x: 5
```

The `5` in `five` is the function’s return value, which is why the return type
is `i32`. Let’s examine this in more detail. There are two important bits:
first, the line `let x = five();` shows that we’re using the return value of a
function to initialize a variable. Because the function `five` returns a `5`,
that line is the same as the following:

```rust
let x = 5;
```

Second, the `five` function has no parameters and defines the type of the
return value, but the body of the function is a lonely `5` with no semicolon
because it’s an expression whose value we want to return. Let’s look at another
example:

<span class="filename">Filename: src/main.rs</span>

```rust
fn main() {
    let x = plus_one(5);

    println!("Значення x: {}", x);
}

fn plus_one(x: i32) -> i32 {
    x + 1
}
```

Running this code will print `Значення x: 6`. What happens if we place a
semicolon at the end of the line containing `x + 1`, changing it from an
expression to a statement?

<span class="filename">Filename: src/main.rs</span>

```rust,ignore
fn main() {
    let x = plus_one(5);

    println!("Значення x: {}", x);
}

fn plus_one(x: i32) -> i32 {
    x + 1;
}
```

Running this code produces an error, as follows:

```text
error[E0269]: not all control paths return a value
 --> src/main.rs:7:1
  |
7 | fn plus_one(x: i32) -> i32 {
  | ^
  |
help: consider removing this semicolon:
 --> src/main.rs:8:10
  |
8 |     x + 1;
  |          ^
```

The main error message, “not all control paths return a value,” reveals the
core issue with this code. The definition of the function `plus_one` says that
it will return an `i32`, but statements don’t evaluate to a value. Therefore,
nothing is returned, which contradicts the function definition and results in
an error. In this output, Rust provides a message to possibly help rectify this
issue: it suggests removing the semicolon, which would fix the error.
