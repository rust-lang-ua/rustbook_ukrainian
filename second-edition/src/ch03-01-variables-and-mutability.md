## Змінні і можливість змінюватися

Я вже згадувалося у Розділі 2, типопо змінні є *сталими* (*immutable*). Це - 
один з численних штурханців, якими Rust заохочує вас писати код, що користується
перевагами у безпеці та швидкості, які надає Rust. Тим не менш, ви все ж маєте 
можливість зробити змінні несталими. Дослідимо, як і чому Rust заохочує вас 
надавати перевагу сталості, та чому ви можете захотіти відмовитися від цього.

Якщо змінна є сталою, це означає, що відколи значення стає прив'язаним до імені,
ви не можете змінити це значення. Для прикладу згенеруємо новий проект з назвою
*variables* ("змінні") у вашій теці *projects* за допомгою 
`cargo new --bin variables`.

Потім, у новоствореній теці *variables*, відкрийте *src/main.rs* і замініть його
код таким:

<span class="filename">Файл: src/main.rs</span>

```rust,ignore
fn main() {
    let x = 5;
    println!("Значення x: {}", x);
    x = 6;
    println!("Значення x: {}", x);
}
```

Збережіть і запустіть програму за допомогою `cargo run`. Ви дістанете 
повідомлення про помилку, як показано тут:

```text
$ cargo run
   Compiling variables v0.0.1 (file:///projects/variables)
error[E0384]: re-assignment of immutable variable `x`
 --> src/main.rs:4:5
  |
2 |     let x = 5;
  |         - first assignment to `x`
3 |     println!("Значення x: {}", x);
4 |     x = 6;
  |     ^^^^^ re-assignment of immutable variable
```

Цей приклад показує, як компілятор допомагає вам знаходити помилки у ваших 
програмах. Хоча повідомлення компілятора про помилки й можуть бути неприємними,
вони лише означають, що ваша програма не ще робить те, що ви хотіли, у безпечний
спосіб; вони *не* означають, що ви поганий програміст! Досвідчені растацеанці
також отримують повідомлення про помилки від компілятора. Повідомлення вказує,
що причиною помилки є "переприсвоєння сталій змінній" (`re-assignment of 
immutable variable`), бо ми намагалися присвоїти друге значення сталій змінній 
`x`.

Важливо, що ми отримали помилку часу компіляції, коли намагалися змінити 
значення, яке раніше визначили як стале, тому що ця ситуація може призвести до
вад у програмі. Якщо одна частина нашого коду працює з припущенням, що значення
не буде змінене, а інша частина нашого коду змінює це значення, можливо, що 
перша частина коду буде робити не те, для чого вона була зроблена. Цю причину 
вад важко відслідкувати після виявлення, особливо коли другий шмат коду змінює
значення лише *іноді*.

У Rust компілятор гарантує, що, якщо ми заявили, що змінна не зміниться, вона і
дійсно не зміниться. Це означає, що коли ви читаєте і пишете код, вам не треба
відстежувати, як і де значення може змінитися, що може полегшити обмірковування
коду.

Але несталість може бути вкрай корисною. Змінні є сталими тільки типово; ми 
можемо зробити їх несталими, додавши `mut` перед іменем змінної. На додачу до
дозволу змінювати це значення, це попереджає майбутніх читачим коду про ваші 
наміри, вказуючи, що інші частини коду будуть змінювати значення цієї змінної.

Наприклад, змінимо *src/main.rs* на такий код:

<span class="filename">Filename: src/main.rs</span>

```rust
fn main() {
    let mut x = 5;
    println!("The value of x is: {}", x);
    x = 6;
    println!("The value of x is: {}", x);
}
```

Запустивши програму, ми отримаємо:

```text
$ cargo run
   Compiling variables v0.1.0 (file:///projects/variables)
     Running `target/debug/variables`
The value of x is: 5
The value of x is: 6
```

Using `mut`, we’re allowed to change the value that `x` binds to from `5` to
`6`. In some cases, you’ll want to make a variable mutable because it makes the
code more convenient to write than an implementation that only uses immutable
variables.

There are multiple trade-offs to consider, in addition to the prevention of
bugs. For example, in cases where you’re using large data structures, mutating
an instance in place may be faster than copying and returning newly allocated
instances. With smaller data structures, always creating new instances and
writing in a more functional programming style may be easier to reason about,
so the lower performance penalty might be worth it to gain that clarity.

### Differences Between Variables and Constants

Not being able to change the value of a variable might have reminded you of
another programming concept that most languages have: *constants*. Constants
are also values bound to a name that are not allowed to change, but there are a
few differences between constants and variables. First, using `mut` with
constants is not allowed: constants aren't only immutable by default, they're
always immutable. Constants are declared using the `const` keyword instead of
the `let` keyword, and the type of the value *must* be annotated. We're about
to cover types and type annotations in the next section, “Data Types,” so don't
worry about the details right now. Constants can be declared in any scope,
including the global scope, which makes them useful for a value that many parts
of your code need to know about. The last difference is that constants may only
be set to a constant expression, not the result of a function call or any other
value that could only be used at runtime.

Here's an example of a constant declaration where the constant's name is
`MAX_POINTS` and its value is set to 100,000. Rust constant naming convention
is to use all upper case with underscores between words:

```
const MAX_POINTS: u32 = 100_000;
```

Constants are valid for the entire lifetime of a program, within the scope they
were declared in. That makes constants useful for values in your application
domain that multiple part of the program might need to know about, such as the
maximum number of points any player of a game is allowed to earn or the number
of seconds in a year.

Documenting hardcoded values used throughout your program by naming them as
constants is useful to convey the meaning of that value to future maintainers
of the code. It also helps to have only one place in your code that you would
need to change if the hardcoded value needed to be updated in the future.

### Shadowing

As we saw in the guessing game tutorial in Chapter 2, we can declare new
variables with the same name as a previous variables, and the new variable
*shadows* the previous variable. Rustaceans say that the first variable is
*shadowed* by the second, which means that the second variable’s value is what
we’ll see when we use the variable. We can shadow a variable by using the same
variable’s name and repeating the use of the `let` keyword as follows:

<span class="filename">Filename: src/main.rs</span>

```rust
fn main() {
    let x = 5;

    let x = x + 1;

    let x = x * 2;

    println!("The value of x is: {}", x);
}
```

This program first binds `x` to a value of `5`. Then it shadows `x` by
repeating `let x =`, taking the original value and adding `1` so the value of
`x` is then `6`. The third `let` statement also shadows `x`, taking the
previous value and multiplying it by `2` to give `x` a final value of `12`.
When you run this program, it will output the following:

```text
$ cargo run
   Compiling variables v0.1.0 (file:///projects/variables)
     Running `target/debug/variables`
The value of x is: 12
```

This is different than marking a variable as `mut`, because unless we use the
`let` keyword again, we’ll get a compile-time error if we accidentally try to
reassign to this variable. We can perform a few transformations on a value but
have the variable be immutable after those transformations have been completed.

The other difference between `mut` and shadowing is that because we’re
effectively creating a new variable when we use the `let` keyword again, we can
change the type of the value, but reuse the same name. For example, say our
program asks a user to show how many spaces they want between some text by
inputting space characters, but we really want to store that input as a number:

```rust
let spaces = "   ";
let spaces = spaces.len();
```

This construct is allowed because the first `spaces` variable is a string type,
and the second `spaces` variable, which is a brand-new variable that happens to
have the same name as the first one, is a number type. Shadowing thus spares us
from having to come up with different names, like `spaces_str` and
`spaces_num`; instead, we can reuse the simpler `spaces` name. However, if we
try to use `mut` for this, as shown here:

```rust,ignore
let mut spaces = "   ";
spaces = spaces.len();
```

we’ll get a compile-time error because we’re not allowed to mutate a variable’s
type:

```text
error[E0308]: mismatched types
 --> src/main.rs:3:14
  |
3 |     spaces = spaces.len();
  |              ^^^^^^^^^^^^ expected &str, found usize
  |
  = note: expected type `&str`
  = note:    found type `usize`
```

Now that we’ve explored how variables work, let’s look at more data types they
can have.
