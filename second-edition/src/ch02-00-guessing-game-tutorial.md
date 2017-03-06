# Гра "відгадай число"

Розпочнемо вивчення Rust зі спільної розробки проекта! Цей розділ ознайомить вас
із кількома поширеними концепціями Rust, показавши вам, як вони використовуються
у реальній програмі. Ви дізнаєтеся про `let`, `match`, методи, асоційовані 
функції, використання зовнішніх ящиків (crate), і навіть більше! Наступні 
розділи розкриють більше деталей цих ідей. У цьому розділі, ви займатиметеся
основами.

Ми розв'язуватимемо класичну задачу для програмістів-початківців: гру "відгадай
число". Умови такі: програма генерує випадкове ціле число між 1 та 100. Потім
пропонує гравцю відгадати. Ввівши спробу, вона скаже, чи число більше або менше
за неї. Якщо відгадано правильно, гра виведе вітання і припиниться.

## Початок нового проекту

Щоб розпочати новий проект, перейдіть до папки *projects*, яку ви створили у 
Розділі 1, і створіть новий проект за допомогою Cargo, ось так:

```text
$ cargo new guessing_game --bin
$ cd guessing_game
```

Перша команда, `cargo new`, приймає першим параметром ім'я проекту 
(`guessing_game`). Прапорець `--bin` каже Cargo зробити двійковий проект, так
само, як і в Розділі 1. Друга команда переходить до теки нового проекту.

Переглянемо щойно створений файл *Cargo.toml*:

<span class="filename">Filename: Cargo.toml</span>

```toml
[package]
name = "guessing_game"
version = "0.1.0"
authors = ["Ваше ім'я <адреса@поштовий.сайт>"]

[dependencies]
```

Якщо інформація про автора, отримана Cargo з вашого середовища, неправильна,
виправіть це і знову збережіть файл.

Як ви вже бачили у Розділі 1, `cargo new` створює програму "Hello, world!". 
Подивимося, що міститься у файлі *src/main.rs*:

<span class="filename">Filename: src/main.rs</span>

```rust
fn main() {
    println!("Hello, world!");
}
```

Скомпілюймо цю програму “Hello, world!” і запустимо її за один крок за допомогою
команди `cargo run`: 

```text
$ cargo run
   Compiling guessing_game v0.1.0 (file:///projects/guessing_game)
     Running `target/debug/guessing_game`
Hello, world!
```

Команда `run` стає в нагоді, коли треба швидко розвивати проект, і ця гра є
якраз таким проектом: ви хочемо швидко тестувати кожну ітерацію перед тим, як
переходити до наступної.

Знову відкрийте файл *src/main.rs*. Весь код ви будете писати у ньому.

## Обробляємо здогадку

Перша частина програми буде запитувати у користувача ввести здогадку, обробляти
те, що він увів, і перевіряти, чи ввів він дані у потрібній формі. Для початку,
дозволимо користувачеві ввести здогадку. Введіть код з Роздруку 2-1 до 
*src/main.rs*.

<figure>
<span class="filename">Filename: src/main.rs</span>

```rust,ignore
use std::io;

fn main() {
    println!("Відгадай число!");

    println!("Введіть здогадку.");

    let mut guess = String::new();

    io::stdin().read_line(&mut guess)
        .expect("Не вдалося прочитати рядок.");

    println!("Ваша здогадка: {}", guess);
}
```

<figcaption>

Роздрук 2-1: Код, що отримує здогадку у користувача і виводить її

</figcaption>
</figure>

Цей код містить багато інформації, тому розбиратимемо його шматок за шматком. 
Щоб отримати, що ввів користувач, і вивести результат, нам треба ввести 
бібліотеку `io` (ввід/вивід, англ. input/output) в область видимості. Бібліотека
`io` входить до стандартної бібліотеки, яка зветься `std`.

```rust,ignore
use std::io;
```

Типово, Rust вводить в область видимості тільки декілька типів у [*прелюдії* (*prelude*)][prelude]<!-- ignore -->. Якщо типу, який ви хочете використати, 
нема у прелюдії, вам доведеться явно вносити цей тип у область видимості за
допомогою виразу `use`.  Використання бібліотеки `std::io` надає вам ряд 
корисних речей, пов'язаних із введенням-виведенням, включно з функціональністю
для користувацького вводу.

[prelude]: https://doc.rust-lang.org/std/prelude/

Як ви бачили у Розділі 1, функція `main` - це точка входу в програму:

```rust,ignore
fn main() {
```

Синтаксична конструкція `fn` проголошує нову функцію, `()` показує, що вона не 
має параметрів, і `{` починає тіло функції.

Як ви вже знаєте з Розділу 1, `println!` - це макрос, що виводить стрічку на
екран:

```rust,ignore
println!("Відгадай число!");

println!("Введіть здогадку.");
```

Цей код просто виводить повідомлення, що це за гра і запит введення у 
користувача.

### Зберігання значень у змінних

Тепер створімо місце для зберігання вводу користувача:

```rust,ignore
let mut guess = String::new();
```

Тепер програма стає цікавішою! В цьому коротенькому рядку відбувається багато
всього. Зверніть увагу, що це оператор `let`, що використовується для створення 
*змінних* (*variable*). Ось інший приклад:

```rust,ignore
let foo = bar;
```

Цей рядок створить нову змінну з назвою `foo` і зв'яже (bind) її зі значенням 
`bar`. У Rust, змінні типово не можуть змінюватися (immutable). Наступний 
приклад показує, як використовується `mut` перед іменем змінної, щоб надати їй 
можливість змінюватися (mutable):

```rust
let foo = 5;     // не може змінюватися
let mut bar = 5; // може змінюватися
```

> Зверніть увагу: синаксична конструкція `//` починає коментар, що продовжується
> до кінця рядка. Rust ігнорує весь вміст коментаря.

Тепео ви знаєте, що `let mut guess` створить змінну, що може змінюватися, на
ім'я `guess`. З іншого боку знаку рівності `=` знаходиться значення, з яким 
зв'язується `guess`, а саме - результат виклику `String::new`, функції, що 
повертає новий екземпляр (instance) стрічки String. [`String`][string]
<!-- ignore --> - це тип стрічки, що надається стандартною бібліотекою; це 
кодовані в UTF-8 шматки тексту, які можна нарощувати.

[string]: ../std/string/struct.String.html

Синаксична конструкція `::` в рядку `::new` позначає, що `new` - це *асоційована 
функція* типу `String`. Асоційована функція є втіленою (implemented) на типі, в
цьому випадку `String`, а не на конкретному екземплярі `String`. В деяких мовах
це зветься *статичним методом*.

This `new` function creates a new, empty `String`. You’ll find a `new` function
on many types, because it’s a common name for a function that makes a new value
of some kind.

To summarize, the `let mut guess = String::new();` line has created a mutable
variable that is currently bound to a new, empty instance of a `String`. Whew!

Recall that we included the input/output functionality from the standard
library with `use std::io;` on the first line of the program. Now we’ll call an
associated function, `stdin`, on `io`:

```rust,ignore
io::stdin().read_line(&mut guess)
    .expect("Failed to read line");
```

If we didn’t have the `use std::io` line at the beginning of the program, we
could have written this function call as `std::io::stdin`. The `stdin` function
returns an instance of [`std::io::Stdin`][iostdin]<!-- ignore -->, which is a
type that represents a handle to the standard input for your terminal.

[iostdin]: ../std/io/struct.Stdin.html

The next part of the code, `.read_line(&mut guess)`, calls the
[`read_line`][read_line]<!-- ignore --> method on the standard input handle to
get input from the user. We’re also passing one argument to `read_line`: `&mut
guess`.

[read_line]: ../std/io/struct.Stdin.html#method.read_line

The job of `read_line` is to take whatever the user types into standard input
and place that into a string, so it takes that string as an argument. The
string argument needs to be mutable so the method can change the string’s
content by adding the user input.

The `&` indicates that this argument is a *reference*, which gives you a way to
let multiple parts of your code access one piece of data without needing to
copy that data into memory multiple times. References are a complex feature,
and one of Rust’s major advantages is how safe and easy it is to use
references. You don’t need to know a lot of those details to finish this
program: Chapter 4 will explain references more thoroughly. For now, all you
need to know is that like variables, references are immutable by default.
Hence, we need to write `&mut guess` rather than `&guess` to make it mutable.

We’re not quite done with this line of code. Although it’s a single line of
text, it’s only the first part of the single logical line of code. The second
part is this method:

```rust,ignore
.expect("Failed to read line");
```

When you call a method with the `.foo()` syntax, it’s often wise to introduce a
newline and other whitespace to help break up long lines. We could have
written this code as:

```rust,ignore
io::stdin().read_line(&mut guess).expect("Failed to read line");
```

However, one long line is difficult to read, so it’s best to divide it, two
lines for two method calls. Now let’s discuss what this line does.

### Handling Potential Failure with the `Result` Type

As mentioned earlier, `read_line` puts what the user types into the string we’re
passing it, but it also returns a value—in this case, an
[`io::Result`][ioresult]<!-- ignore -->. Rust has a number of types named
`Result` in its standard library: a generic [`Result`][result]<!-- ignore --> as
well as specific versions for submodules, such as `io::Result`.

[ioresult]: ../std/io/type.Result.html
[result]: ../std/result/enum.Result.html

The `Result` types are [*enumerations*][enums]<!-- ignore -->, often referred
to as *enums*. An enumeration is a type that can have a fixed set of values,
and those values are called the enum’s *variants*. Chapter 6 will cover enums
in more detail.

[enums]: ch06-00-enums.html

For `Result`, the variants are `Ok` or `Err`. `Ok` indicates the operation was
successful, and inside the `Ok` variant is the successfully generated value.
`Err` means the operation failed, and `Err` contains information about how or
why the operation failed.

The purpose of these `Result` types is to encode error handling information.
Values of the `Result` type, like any type, have methods defined on them. An
instance of `io::Result` has an [`expect` method][expect]<!-- ignore --> that
you can call. If this instance of `io::Result` is an `Err` value, `expect` will
cause the program to crash and display the message that you passed as an
argument to `expect`. If the `read_line` method returns an `Err`, it would
likely be the result of an error coming from the underlying operating system.
If this instance of `io::Result` is an `Ok` value, `expect` will take the
return value that `Ok` is holding and return just that value to you so you
could use it. In this case, that value is the number of characters the user
entered into standard input.

[expect]: ../std/result/enum.Result.html#method.expect

If we don’t call `expect`, the program will compile, but we’ll get a warning:

```text
$ cargo build
   Compiling guessing_game v0.1.0 (file:///projects/guessing_game)
src/main.rs:10:5: 10:39 warning: unused result which must be used,
#[warn(unused_must_use)] on by default
src/main.rs:10     io::stdin().read_line(&mut guess);
                   ^~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
```

Rust warns that we haven’t used the `Result` value returned from `read_line`,
indicating that the program hasn’t handled a possible error. The right way to
suppress the warning is to actually write error handling, but since we just
want to crash this program when a problem occurs, we can use `expect`. You’ll
learn about recovering from errors in Chapter 9.

### Printing Values with `println!` Placeholders

Aside from the closing curly brace, there’s only one more line to discuss in
the code added so far, which is the following:

```rust,ignore
println!("You guessed: {}", guess);
```

This line prints out the string we saved the user’s input in. The set of `{}`
is a placeholder that holds a value in place. You can print more than one value
using `{}`: the first set of `{}` holds the first value listed after the format
string, the second set holds the second value, and so on. Printing out multiple
values in one call to `println!` would look like this:

```rust
let x = 5;
let y = 10;

println!("x = {} and y = {}", x, y);
```

This code would print out `x = 5 and y = 10`.

### Testing the First Part

Let’s test the first part of the guessing game. You can run it using `cargo run`:

```text
$ cargo run
   Compiling guessing_game v0.1.0 (file:///projects/guessing_game)
     Running `target/debug/guessing_game`
Guess the number!
Please input your guess.
6
You guessed: 6
```

At this point, the first part of the game is done: we’re getting input from the
keyboard and then printing it.

## Generating a Secret Number

Next, we need to generate a secret number that the user will try to guess. The
secret number should be different every time so the game is fun to play more
than once. Let’s use a random number between 1 and 100 so the game isn’t too
difficult. Rust doesn’t yet include random number functionality in its standard
library. However, the Rust team does provide a [`rand` crate][randcrate].

[randcrate]: https://crates.io/crates/rand

### Using a Crate to Get More Functionality

Remember that a *crate* is a package of Rust code. The project we’ve been
building is a *binary crate*, which is an executable. The `rand` crate is a
*library crate*, which contains code intended to be used in other programs.

Cargo’s use of external crates is where it really shines. Before we can write
code that uses `rand`, we need to modify the *Cargo.toml* file to include the
`rand` crate as a dependency. Open that file now and add the following line to
the bottom beneath the `[dependencies]` section header that Cargo created for
you:

<span class="filename">Filename: Cargo.toml</span>

```toml
[dependencies]

rand = "0.3.14"
```

In the *Cargo.toml* file, everything that follows a header is part of a section
that continues until another section starts. The `[dependencies]` section is
where you tell Cargo which external crates your project depends on and which
versions of those crates you require. In this case, we’ll specify the `rand`
crate with the semantic version specifier `0.3.14`. Cargo understands [Semantic
Versioning][semver]<!-- ignore --> (sometimes called *SemVer*), which is a
standard for writing version numbers. The number `0.3.14` is actually shorthand
for `^0.3.14`, which means “any version that has a public API compatible with
version 0.3.14.”

[semver]: http://semver.org

Now, without changing any of the code, let’s build the project, as shown in
Listing 2-2:

<figure>

```text
$ cargo build
    Updating registry `https://github.com/rust-lang/crates.io-index`
 Downloading rand v0.3.14
 Downloading libc v0.2.14
   Compiling libc v0.2.14
   Compiling rand v0.3.14
   Compiling guessing_game v0.1.0 (file:///projects/guessing_game)
```

<figcaption>

Listing 2-2: The output from running `cargo build` after adding the rand crate
as a dependency

</figcaption>
</figure>

You may see different version numbers (but they will all be compatible with
the code, thanks to SemVer!), and the lines may be in a different order.

Now that we have an external dependency, Cargo fetches the latest versions of
everything from the *registry*, which is a copy of data from
[Crates.io][cratesio]. Crates.io is where people in the Rust ecosystem post
their open source Rust projects for others to use.

[cratesio]: https://crates.io

After updating the registry, Cargo checks the `[dependencies]` section and
downloads any you don’t have yet. In this case, although we only listed `rand`
as a dependency, Cargo also grabbed a copy of `libc`, because `rand` depends on
`libc` to work. After downloading them, Rust compiles them and then compiles
the project with the dependencies available.

If you immediately run `cargo build` again without making any changes, you won’t
get any output. Cargo knows it has already downloaded and compiled the
dependencies, and you haven't changed anything about them in your *Cargo.toml*
file. Cargo also knows that you haven't changed anything about your code, so it
doesn't recompile that either. With nothing to do, it simply exits. If you open
up the *src/main.rs* file, make a trivial change, then save it and build again,
you’ll only see one line of output:

```text
$ cargo build
   Compiling guessing_game v0.1.0 (file:///projects/guessing_game)
```

This line shows Cargo only updates the build with your tiny change to the
*src/main.rs* file. Your dependencies haven't changed, so Cargo knows it can
reuse what it has already downloaded and compiled for those. It just rebuilds
your part of the code.

#### The *Cargo.lock* File Ensures Reproducible Builds

Cargo has a mechanism that ensures you can rebuild the same artifact every time
you or anyone else builds your code: Cargo will use only the versions of the
dependencies you specified until you indicate otherwise. For example, what
happens if next week version `v0.3.15` of the `rand` crate comes out and
contains an important bug fix but also contains a regression that will break
your code?

The answer to this problem is the *Cargo.lock* file, which was created the
first time you ran `cargo build` and is now in your *guessing_game* directory.
When you build a project for the first time, Cargo figures out all the
versions of the dependencies that fit the criteria and then writes them to
the *Cargo.lock* file. When you build your project in the future, Cargo will
see that the *Cargo.lock* file exists and use the versions specified there
rather than doing all the work of figuring out versions again. This lets you
have a reproducible build automatically. In other words, your project will
remain at `0.3.14` until you explicitly upgrade, thanks to the *Cargo.lock*
file.

#### Updating a Crate to Get a New Version

When you *do* want to update a crate, Cargo provides another command, `update`,
which will:

1. Ignore the *Cargo.lock* file and figure out all the latest versions that fit
your specifications in *Cargo.toml*.
1. If that works, Cargo will write those versions to the *Cargo.lock* file.

But by default, Cargo will only look for versions larger than `0.3.0` and
smaller than `0.4.0`. If the `rand` crate has released two new versions,
`0.3.15` and `0.4.0`, you would see the following if you ran `cargo update`:

```text
$ cargo update
    Updating registry `https://github.com/rust-lang/crates.io-index`
    Updating rand v0.3.14 -> v0.3.15
```

At this point, you would also notice a change in your *Cargo.lock* file noting
that the version of the `rand` crate you are now using is `0.3.15`.

If you wanted to use `rand` version `0.4.0` or any version in the `0.4.x`
series, you’d have to update the *Cargo.toml* file to look like this instead:

```toml
[dependencies]

rand = "0.4.0"
```

The next time you run `cargo build`, Cargo will update the registry of crates
available and reevaluate your `rand` requirements according to the new version
you specified.

There’s a lot more to say about [Cargo][doccargo]<!-- ignore --> and [its
ecosystem][doccratesio]<!-- ignore --> that Chapter 14 will discuss, but for
now, that’s all you need to know. Cargo makes it very easy to reuse libraries,
so Rustaceans are able to write smaller projects that are assembled from a
number of packages.

[doccargo]: http://doc.crates.io
[doccratesio]: http://doc.crates.io/crates-io.html

### Generating a Random Number

Let’s start *using* `rand`. The next step is to update *src/main.rs*, as shown
in Listing 2-3:

<figure>
<span class="filename">Filename: src/main.rs</span>

```rust,ignore
extern crate rand;

use std::io;
use rand::Rng;

fn main() {
    println!("Guess the number!");

    let secret_number = rand::thread_rng().gen_range(1, 101);

    println!("The secret number is: {}", secret_number);

    println!("Please input your guess.");

    let mut guess = String::new();

    io::stdin().read_line(&mut guess)
        .expect("Failed to read line");

    println!("You guessed: {}", guess);
}
```

<figcaption>

Listing 2-3: Code changes needed in order to generate a random number

</figcaption>
</figure>

We’re adding a `extern crate rand;` line to the top that lets Rust know we’ll be
using that external dependency. This also does the equivalent of calling `use
rand`, so now we can call anything in the `rand` crate by prefixing it with
`rand::`.

Next, we’re adding another `use` line: `use rand::Rng`. `Rng` is a trait that
defines methods that random number generators implement, and this trait must be
in scope for us to use those methods. Chapter 10 will cover traits in detail.

Also, we’re adding two more lines in the middle. The `rand::thread_rng` function
will give us the particular random number generator that we’re going to use:
one that is local to the current thread of execution and seeded by the
operating system. Next, we call the `gen_range` method on the random number
generator. This method is defined by the `Rng` trait that we brought into
scope with the `use rand::Rng` statement. The `gen_range` method takes two
numbers as arguments and generates a random number between them. It’s inclusive
on the lower bound but exclusive on the upper bound, so we need to specify `1`
and `101` to request a number between 1 and 100.

Knowing which traits to use and which functions and methods to call from a
crate isn’t something that you’ll just *know*. Instructions for using a crate
are in each crate’s documentation. Another neat feature of Cargo is that you
can run the `cargo doc --open` command that will build documentation provided
by all of your dependencies locally and open it in your browser. If you’re
interested in other functionality in the `rand` crate, for example, run `cargo
doc --open` and click `rand` in the sidebar on the left.

The second line that we added to the code prints the secret number. This is
useful while we’re developing the program to be able to test it, but we’ll
delete it from the final version. It’s not much of a game if the program prints
the answer as soon as it starts!

Try running the program a few times:

```text
$ cargo run
   Compiling guessing_game v0.1.0 (file:///projects/guessing_game)
     Running `target/debug/guessing_game`
Guess the number!
The secret number is: 7
Please input your guess.
4
You guessed: 4
$ cargo run
     Running `target/debug/guessing_game`
Guess the number!
The secret number is: 83
Please input your guess.
5
You guessed: 5
```

You should get different random numbers, and they should all be numbers between
1 and 100. Great job!

## Comparing the Guess to the Secret Number

Now that we have user input and a random number, we can compare them. That
step is shown in Listing 2-4:

<figure>
<span class="filename">Filename: src/main.rs</span>

```rust,ignore
extern crate rand;

use std::io;
use std::cmp::Ordering;
use rand::Rng;

fn main() {
    println!("Guess the number!");

    let secret_number = rand::thread_rng().gen_range(1, 101);

    println!("The secret number is: {}", secret_number);

    println!("Please input your guess.");

    let mut guess = String::new();

    io::stdin().read_line(&mut guess)
        .expect("Failed to read line");

    println!("You guessed: {}", guess);

    match guess.cmp(&secret_number) {
        Ordering::Less    => println!("Too small!"),
        Ordering::Greater => println!("Too big!"),
        Ordering::Equal   => println!("You win!"),
    }
}
```

<figcaption>

Listing 2-4: Handling the possible return values of comparing two numbers

</figcaption>
</figure>

The first new bit here is another `use`, bringing a type called
`std::cmp::Ordering` into scope from the standard library. `Ordering` is
another enum, like `Result`, but the variants for `Ordering` are `Less`,
`Greater`, and `Equal`. These are the three outcomes that are possible when you
compare two values.

Then we add five new lines at the bottom that use the `Ordering` type:

```rust,ignore
match guess.cmp(&secret_number) {
    Ordering::Less    => println!("Too small!"),
    Ordering::Greater => println!("Too big!"),
    Ordering::Equal   => println!("You win!"),
}
```

The `cmp` method compares two values and can be called on anything that can be
compared. It takes a reference to whatever you want to compare with: here it’s
comparing the `guess` to the `secret_number`. `cmp` returns a variant of the
`Ordering` enum we brought into scope with the `use` statement. We use a
[`match`][match]<!-- ignore --> expression to decide what to do next based on
which variant of `Ordering` was returned from the call to `cmp` with the values
in `guess` and `secret_number`.

[match]: ch06-02-match.html

A `match` expression is made up of *arms*. An arm consists of a *pattern* and
the code that should be run if the value given to the beginning of the `match`
expression fits that arm’s pattern. Rust takes the value given to `match` and
looks through each arm’s pattern in turn. The `match` construct and patterns
are powerful features in Rust that let you express a variety of situations your
code might encounter and helps ensure that you handle them all. These features
will be covered in detail in Chapter 6 and Chapter 19, respectively.

Let’s walk through an example of what would happen with the `match` expression
used here. Say that the user has guessed 50, and the randomly generated secret
number this time is 38. When the code compares 50 to 38, the `cmp` method will
return `Ordering::Greater`, because 50 is greater than 38. `Ordering::Greater`
is the value that the `match` expression gets. It looks at the first arm’s
pattern, `Ordering::Less`, but the value `Ordering::Greater` does not match
`Ordering::Less`. So it ignores the code in that arm and moves to the next arm.
The next arm’s pattern, `Ordering::Greater`, *does* match
`Ordering::Greater`! The associated code in that arm will execute and print
`Too big!` to the screen. The `match` expression ends because it has no need to
look at the last arm in this particular scenario.

However, the code in Listing 2-4 won’t compile yet. Let’s try it:

```text
$ cargo build
   Compiling guessing_game v0.1.0 (file:///projects/guessing_game)
error[E0308]: mismatched types
  --> src/main.rs:23:21
   |
23 |     match guess.cmp(&secret_number) {
   |                     ^^^^^^^^^^^^^^ expected struct `std::string::String`, found integral variable
   |
   = note: expected type `&std::string::String`
   = note:    found type `&{integer}`

error: aborting due to previous error
Could not compile `guessing_game`.
```

The core of the error states that there are *mismatched types*. Rust has a
strong, static type system. However, it also has type inference. When we wrote
`let guess = String::new()`, Rust was able to infer that `guess` should be a
`String` and didn’t make us write the type. The `secret_number`, on the other
hand, is a number type. A few number types can have a value between 1 and 100:
`i32`, a 32-bit number; `u32`, an unsigned 32-bit number; `i64`, a 64-bit
number; as well as others. Rust defaults to an `i32`, which is the type of
`secret_number` unless we add type information elsewhere that would cause Rust
to infer a different numerical type. The reason for the error is that Rust will
not compare a string and a number type.

Ultimately, we want to convert the `String` the program reads as input into a
real number type so we can compare it to the guess numerically. We can do
that by adding the following two lines to the `main` function body:

<span class="filename">Filename: src/main.rs</span>

```rust,ignore
extern crate rand;

use std::io;
use std::cmp::Ordering;
use rand::Rng;

fn main() {
    println!("Guess the number!");

    let secret_number = rand::thread_rng().gen_range(1, 101);

    println!("The secret number is: {}", secret_number);

    println!("Please input your guess.");

    let mut guess = String::new();

    io::stdin().read_line(&mut guess)
        .expect("Failed to read line");

    let guess: u32 = guess.trim().parse()
        .expect("Please type a number!");

    println!("You guessed: {}", guess);

    match guess.cmp(&secret_number) {
        Ordering::Less    => println!("Too small!"),
        Ordering::Greater => println!("Too big!"),
        Ordering::Equal   => println!("You win!"),
    }
}
```

The two new lines are:

```rust,ignore
let guess: u32 = guess.trim().parse()
    .expect("Please type a number!");
```

We create a variable named `guess`. But wait, doesn’t the program
already have a variable named `guess`? It does, but Rust allows us to
*shadow* the previous value of `guess` with a new one. This feature is often
used in similar situations in which you want to convert a value from one type
to another type. Shadowing lets us reuse the `guess` variable name rather than
forcing us to create two unique variables, like `guess_str` and `guess` for
example. (Chapter 3 covers shadowing in more detail.)

We bind `guess` to the expression `guess.trim().parse()`. The `guess` in the
expression refers to the original `guess` that was a `String` with the input in
it. The `trim` method on a `String` instance will eliminate any whitespace at
the beginning and end. `u32` can only contain numerical characters, but the
user must press the Return key to satisfy `read_line`. When the user presses
Return, a newline character is added to the string. For example, if the user
types 5 and presses return, `guess` looks like this: `5\n`. The `\n` represents
“newline,” the return key. The `trim` method eliminates `\n`, resulting in just
`5`.

The [`parse` method on strings][parse]<!-- ignore --> parses a string into some
kind of number. Because this method can parse a variety of number types, we
need to tell Rust the exact number type we want by using `let guess: u32`. The
colon (`:`) after `guess` tells Rust we’ll annotate the variable’s type. Rust
has a few built-in number types; the `u32` seen here is an unsigned, 32-bit
integer. It’s a good default choice for a small positive number. You’ll learn
about other number types in Chapter 3. Additionally, the `u32` annotation in
this example program and the comparison with `secret_number` means that Rust
will infer that `secret_number` should be a `u32` as well. So now the
comparison will be between two values of the same type!

[parse]: ../std/primitive.str.html#method.parse

The call to `parse` could easily cause an error. If, for example, the string
contained `A👍%`, there would be no way to convert that to a number. Because it
might fail, the `parse` method returns a `Result` type, much like the
`read_line` method does as discussed earlier in “Handling Potential Failure
with the Result Type” on page XX. We’ll treat this `Result` the same way by
using the `expect` method again. If `parse` returns an `Err` `Result` variant
because it couldn’t create a number from the string, the `expect` call will
crash the game and print the message we give it. If `parse` can successfully
convert the string to a number, it will return the `Ok` variant of `Result`,
and `expect` will return the number that we want from the `Ok` value.

Let’s run the program now!

```text
$ cargo run
   Compiling guessing_game v0.1.0 (file:///projects/guessing_game)
     Running `target/guessing_game`
Guess the number!
The secret number is: 58
Please input your guess.
  76
You guessed: 76
Too big!
```

Nice! Even though spaces were added before the guess, the program still figured
out that the user guessed 76. Run the program a few times to verify the
different behavior with different kinds of input: guess the number correctly,
guess a number that is too high, and guess a number that is too low.

We have most of the game working now, but the user can make only one guess.
Let’s change that by adding a loop!

## Allowing Multiple Guesses with Looping

The `loop` keyword gives us an infinite loop. Add that now to give users more
chances at guessing the number:

<span class="filename">Filename: src/main.rs</span>

```rust,ignore
extern crate rand;

use std::io;
use std::cmp::Ordering;
use rand::Rng;

fn main() {
    println!("Guess the number!");

    let secret_number = rand::thread_rng().gen_range(1, 101);

    println!("The secret number is: {}", secret_number);

    loop {
        println!("Please input your guess.");

        let mut guess = String::new();

        io::stdin().read_line(&mut guess)
            .expect("Failed to read line");

        let guess: u32 = guess.trim().parse()
            .expect("Please type a number!");

        println!("You guessed: {}", guess);

        match guess.cmp(&secret_number) {
            Ordering::Less    => println!("Too small!"),
            Ordering::Greater => println!("Too big!"),
            Ordering::Equal   => println!("You win!"),
        }
    }
}
```

As you can see, we’ve moved everything into a loop from the guess input prompt
onward. Be sure to indent those lines another four spaces each, and run the
program again. Notice that there is a new problem because the program is doing
exactly what we told it to do: ask for another guess forever! It doesn’t seem
like the user can quit!

The user could always halt the program by using the keyboard shortcut `Ctrl-C`.
But there’s another way to escape this insatiable monster that we mentioned in
the `parse` discussion in “Comparing the Guesses” on page XX: if the user
enters a non-number answer, the program will crash. The user can take advantage
of that in order to quit, as shown here:

```text
$ cargo run
   Compiling guessing_game v0.1.0 (file:///projects/guessing_game)
     Running `target/guessing_game`
Guess the number!
The secret number is: 59
Please input your guess.
45
You guessed: 45
Too small!
Please input your guess.
60
You guessed: 60
Too big!
Please input your guess.
59
You guessed: 59
You win!
Please input your guess.
quit
thread 'main' panicked at 'Please type a number!: ParseIntError { kind: InvalidDigit }', src/libcore/result.rs:785
note: Run with `RUST_BACKTRACE=1` for a backtrace.
error: Process didn't exit successfully: `target/debug/guess` (exit code: 101)
```

Typing `quit` actually quits the game, but so will any other non-number input.
However, this is suboptimal to say the least. We want the game to automatically
stop when the correct number is guessed.

### Quitting After a Correct Guess

Let’s program the game to quit when the user wins by adding a `break`:

<span class="filename">Filename: src/main.rs</span>

```rust,ignore
extern crate rand;

use std::io;
use std::cmp::Ordering;
use rand::Rng;

fn main() {
    println!("Guess the number!");

    let secret_number = rand::thread_rng().gen_range(1, 101);

    println!("The secret number is: {}", secret_number);

    loop {
        println!("Please input your guess.");

        let mut guess = String::new();

        io::stdin().read_line(&mut guess)
            .expect("Failed to read line");

        let guess: u32 = guess.trim().parse()
            .expect("Please type a number!");

        println!("You guessed: {}", guess);

        match guess.cmp(&secret_number) {
            Ordering::Less    => println!("Too small!"),
            Ordering::Greater => println!("Too big!"),
            Ordering::Equal   => {
                println!("You win!");
                break;
            }
        }
    }
}
```

By adding the `break` line after `You win!`, the program will exit the loop
when the user guesses the secret number correctly. Exiting the loop also means
exiting the program, because the loop is the last part of `main`.

### Handling Invalid Input

To further refine the game’s behavior, rather than crashing the program when
the user inputs a non-number, let’s make the game ignore a non-number so the
user can continue guessing. We can do that by altering the line where `guess` is
converted from a `String` to a `u32`:

```rust,ignore
let guess: u32 = match guess.trim().parse() {
    Ok(num) => num,
    Err(_) => continue,
};
```

Switching from an `expect` call to a `match` expression is how you generally
move from crash on error to actually handling the error. Remember that `parse`
returns a `Result` type, and `Result` is an enum that has the variants `Ok` or
`Err`. We’re using a `match` expression here, like we did with the `Ordering`
result of the `cmp` method.

If `parse` is able to successfully turn the string into a number, it will return
an `Ok` value that contains the resulting number. That `Ok` value will match the
first arm’s pattern, and the `match` expression will just return the `num` value
that `parse` produced and put inside the `Ok` value. That number will end up
right where we want it in the new `guess` variable we’re creating.

If `parse` is *not* able to turn the string into a number, it will return an
`Err` value that contains more information about the error. The `Err` value
does not match the `Ok(num)` pattern in the first `match` arm, but it does match
the `Err(_)` pattern in the second arm. The `_` is a catchall value; in this
example, we’re saying we want to match all `Err` values, no matter what
information they have inside them. So the program will execute the second arm’s
code, `continue`, which means to go to the next iteration of the `loop` and ask
for another guess. So effectively, the program ignores all errors that `parse`
might encounter!

Now everything in the program should work as expected. Let’s try it by running
`cargo run`:

```text
$ cargo run
   Compiling guessing_game v0.1.0 (file:///projects/guessing_game)
     Running `target/guessing_game`
Guess the number!
The secret number is: 61
Please input your guess.
10
You guessed: 10
Too small!
Please input your guess.
99
You guessed: 99
Too big!
Please input your guess.
foo
Please input your guess.
61
You guessed: 61
You win!
```

Awesome! With one tiny final tweak, we will finish the guessing game: recall
that the program is still printing out the secret number. That worked well for
testing, but it ruins the game. Let’s delete the `println!` that outputs the
secret number. Listing 2-5 shows the final code:

<figure>
<span class="filename">Filename: src/main.rs</span>

```rust,ignore
extern crate rand;

use std::io;
use std::cmp::Ordering;
use rand::Rng;

fn main() {
    println!("Guess the number!");

    let secret_number = rand::thread_rng().gen_range(1, 101);

    loop {
        println!("Please input your guess.");

        let mut guess = String::new();

        io::stdin().read_line(&mut guess)
            .expect("Failed to read line");

        let guess: u32 = match guess.trim().parse() {
            Ok(num) => num,
            Err(_) => continue,
        };

        println!("You guessed: {}", guess);

        match guess.cmp(&secret_number) {
            Ordering::Less    => println!("Too small!"),
            Ordering::Greater => println!("Too big!"),
            Ordering::Equal   => {
                println!("You win!");
                break;
            }
        }
    }
}
```

<figcaption>

Listing 2-5: Complete code of the guessing game

</figcaption>
</figure>

## Summary

At this point, you’ve successfully built the guessing game! Congratulations!

This project was a hands-on way to introduce you to many new Rust concepts:
`let`, `match`, methods, associated functions, using external crates, and more.
In the next few chapters, you’ll learn about these concepts in more detail.
Chapter 3 covers concepts that most programming languages have, such as
variables, data types, and functions, and shows how to use them in Rust.
Chapter 4 explores ownership, which is a Rust feature that is most different
from other languages. Chapter 5 discusses structs and method syntax, and
Chapter 6 endeavors to explain enums.
