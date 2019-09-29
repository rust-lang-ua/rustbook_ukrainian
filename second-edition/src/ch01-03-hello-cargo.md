## Привіт, Cargo!

Cargo - це система побудови та пакетний менеджер. Більшість Растацеанців 
використовуватимуть цей інструмент для керування проєктами Rust, бо Cargo 
виконує багато задач, таких, як побудова коду, завантаження бібліотек, від яких залежить ваш код, та побудова цих бібліотек (бібліотеки, потрібні коду,
звуться *залежностями* (*dependencies*).

Найпростіші програми Rust, як та, яку ми щойно написали, не мають жодних 
залежностей, тому, якби ми побудували проєкт Hello world за допомогою Cargo,
то скористалися б тільки тією частиною Cargo, що відповідає з побудову коду. 
Якщо ж писати складніші програми Rust, виникне потреба в залежностях, і якщо 
почати проєкт за допомогою Cargo, задовольнити її буде значно легше.

Оскільки переважна більшість проєктів Rust використовують Cargo, надалі в книзі
вважатиметься, що ви теж використовуєте Cargo. Cargo встановлюється з Rust, 
якщо ви скористалися офіційним встановлювачем, як сказано в розділі 
“Встановлення”. Якщо ви встановили Rust у інший спосіб, можете перевірити, чи 
встановлений Cargo, ввівши це у свій термінал:

```text
$ cargo --version
```

Якщо ви побачите номер версії, то чудово! Але якщо ви бачите помилку на кшталт
`не знайдено команду`, вам слід звернутися до документації по вашому методу
встановлення, щоб визначити, як окремо встановити Cargo.

### Створення проєкту за допомогою Cargo

Створімо новий проєкт за допомогою Cargo і подивимося, як він відрізняється від
нашого початкового проєкту Hello World. Поверніться до вашої теки *projects* (чи іншої, де знаходиться ваш код) і введіть команди (незалежно від системи):

```text
$ cargo new hello_cargo --bin
$ cd hello_cargo
```

<!-- Below -- so we always have to start a cargo project with the --bin option
if we want it to be something we can execute and not just a library, is that
right? It might be worth laying that out -->
<!-- As of Rust 1.21.0 (the version we're using for the book), yes, you must
always specify `--bin`. In a version of Rust in the near future (1.25 or 1.26),
binary crates will become the default kind of crate that `cargo new` makes, so
you won't have to specify `--bin` (but you can if you want and the behavior
will be the same). We'd rather not go into any more detail than we have here
because of this change; I think "The `--bin` argument to passed to `cargo new`
makes an executable application (often just called a *binary*), as opposed to a
library." lays this out enough. /Carol -->

Це створює новий двійковий виконанний проєкт, що зветься hello_cargo. Аргумент
`--bin`, переданий до `cargo new`, створює виконанний застосунок (який часто 
просто називають *двійковим файлом*); натомість, він міг створити бібліотеку.
Ми ввели `hello_cargo` як назву нашого проєкту, і Cargo створює його файли у 
теці, що зветься так само.

Перейдіть до теки *hello_cargo* і перегляньте файли, і ви побачите, що Cargo 
створив два файли і одну теку: *Cargo.toml* і теку *src* із файлом *main.rs* 
Також він розпочав новий репозиторій git, додавши файл *.gitignore*.

> Примітка: Git - це поширена система контролю версій (version control system, 
> VCS). Ви можете сказати `cargo new` використовувати іншу систему контрою 
> версій чи не використовувати жодної за допомогою прапорця `--vcs`. Запустіть
> `cargo new --help`, щоб побачити можливі варіанти.

Відкрийте файл *Cargo.toml* у будь-якому текстовому редакторі. Він має 
виглядати десь так, як показано у Роздруку 1-2:

<span class="filename">Файл: Cargo.toml</span>

```toml
[package]
name = "hello_cargo"
version = "0.1.0"
authors = ["Ваше Ім'я <email@example.com>"]

[dependencies]
```

<span class="caption">Роздрук 1-2: Вміст файлу *Cargo.toml*, створеного 
командою `cargo new`</span>

Це файл у форматі [*TOML*][toml]<!-- ignore --> (Tom’s Obvious, Minimal
Language - "томова очевидна мінімальна мова"), який Cargo використовує як 
формат для конфігурації.

[toml]: https://github.com/toml-lang/toml

Перший рядок, `[package]` (пакет), це заголовок розділу, що показує, що 
наступні інструкції стосуються конфігурації пакету. Коли ми додамо більше 
інформації до цього файлу, ми додамо й інші розділи.

Наступні три рядки встановлюють конфігураційну інформацію, потрібну Cargo для
компілювання вашої програми: ім'я, версію, і хто її написав. Cargo бере ваше 
ім'я та адресу електронної пошти з налаштувань середовища, і якщо вони 
неправильні, можете їх виправити та зберегти файл.

Останній рядок, `[dependencies]`, розпочинає розділ, де можна вказувати 
залежності вашого проєкту. Пакети з кодом в Rust звуться *ящиками* (*Crates*).
Нам не знадобиться інших ящиків для цього проєкту, але вони знадобляться для
першого проєкту у розділі 2, і тоді ж ми скористаємося цим розділом.

Тепер відкрийте файл *src/main.rs* і подивіться на його вміст:

<span class="filename">Файл: src/main.rs</span>

```rust
fn main() {
    println!("Hello, world!");
}
```

Cargo has generated a “Hello World!” for you, just like the one we wrote in
Listing 1-1! So far, the differences between our previous project and the
project generated by Cargo are that with Cargo our code goes in the *src*
directory, and we have a *Cargo.toml* configuration file in the top directory.

Cargo expects your source files to live inside the *src* directory so that the
top-level project directory is just for READMEs, license information,
configuration files, and anything else not related to your code. In this way,
using Cargo helps you keep your projects nice and tidy. There’s a place for
everything, and everything is in its place.

If you started a project that doesn’t use Cargo, as we did with our project in
the *hello_world* directory, you can convert it to a project that does use
Cargo by moving the project code into the *src* directory and creating an
appropriate *Cargo.toml*.

### Building and Running a Cargo Project

Now let’s look at what’s different about building and running your Hello World
program through Cargo! From your project directory, build your project by
entering the following commands:

```text
$ cargo build
   Compiling hello_cargo v0.1.0 (file:///projects/hello_cargo)
    Finished dev [unoptimized + debuginfo] target(s) in 2.85 secs
```

This creates an executable file in *target/debug/hello_cargo* (or
*target\\debug\\hello_cargo.exe* on Windows), which you can run with this
command:

```text
$ ./target/debug/hello_cargo # or .\target\debug\hello_cargo.exe on Windows
Hello, world!
```

Bam! If all goes well, `Hello, world!` should print to the terminal once more.
Running `cargo build` for the first time also causes Cargo to create a new file
at the top level called *Cargo.lock*, which is used to keep track of the exact
versions of dependencies in your project. This project doesn’t have
dependencies, so the file is a bit sparse. You won’t ever need to touch this
file yourself; Cargo will manage its contents for you.

We just built a project with `cargo build` and ran it with
`./target/debug/hello_cargo`, but we can also use `cargo run` to compile and
then run all in one go:

```text
$ cargo run
    Finished dev [unoptimized + debuginfo] target(s) in 0.0 secs
     Running `target/debug/hello_cargo`
Hello, world!
```

Notice that this time, we didn’t see the output telling us that Cargo was
compiling `hello_cargo`. Cargo figured out that the files haven’t changed, so
it just ran the binary. If you had modified your source code, Cargo would have
rebuilt the project before running it, and you would have seen output like this:

```text
$ cargo run
   Compiling hello_cargo v0.1.0 (file:///projects/hello_cargo)
    Finished dev [unoptimized + debuginfo] target(s) in 0.33 secs
     Running `target/debug/hello_cargo`
Hello, world!
```

Finally, there’s `cargo check`. This command will quickly check your code to
make sure that it compiles, but not bother producing an executable:

```text
$ cargo check
   Compiling hello_cargo v0.1.0 (file:///projects/hello_cargo)
    Finished dev [unoptimized + debuginfo] target(s) in 0.32 secs
```

Why would you not want an executable? `cargo check` is often much faster than
`cargo build`, because it skips the entire step of producing the executable. If
you’re checking your work throughout the process of writing the code, using
`cargo check` will speed things up! As such, many Rustaceans run `cargo check`
periodically as they write their program to make sure that it compiles, and
then run `cargo build` once they’re ready to give it a spin themselves.

So to recap, using Cargo:

- We can build a project using `cargo build` or `cargo check`
- We can build and run the project in one step with `cargo run`
- Instead of the result of the build being put in the same directory as our
  code, Cargo will put it in the *target/debug* directory.

A final advantage of using Cargo is that the commands are the same no matter
what operating system you’re on, so at this point we will no longer be
providing specific instructions for Linux and Mac versus Windows.

### Building for Release

When your project is finally ready for release, you can use `cargo build
--release` to compile your project with optimizations. This will create an
executable in *target/release* instead of *target/debug*. These optimizations
make your Rust code run faster, but turning them on makes your program take
longer to compile. This is why there are two different profiles: one for
development when you want to be able to rebuild quickly and often, and one for
building the final program you’ll give to a user that won’t be rebuilt
repeatedly and that will run as fast as possible. If you’re benchmarking the
running time of your code, be sure to run `cargo build --release` and benchmark
with the executable in *target/release*.

### Cargo as Convention

With simple projects, Cargo doesn’t provide a whole lot of value over just
using `rustc`, but it will prove its worth as you continue. With complex
projects composed of multiple crates, it’s much easier to let Cargo coordinate
the build.

Even though the `hello_cargo` project is simple, it now uses much of the real
tooling you’ll use for the rest of your Rust career. In fact, to work on any
existing projects you can use the following commands to check out the code
using Git, change into the project directory, and build:

```text
$ git clone someurl.com/someproject
$ cd someproject
$ cargo build
```

If you want to look at Cargo in more detail, check out [its documentation].

[its documentation]: https://doc.rust-lang.org/cargo/

<!--Below -- I`m not sure this is the place for this conversation, it seems too
deep into the weeds for the "getting started" chapter. I know we discussed
Nightly Rust as an appendix previously, but honestly I think this is more
suited somewhere online, perhaps in the extended docs. I like the idea of
finishing the chapter here, on this practical note, and I think at this point
readers will want to get stuck in anyway and may skip this and never come back
because it's buried at the end of a chapter that's not really related to it. If
it's online somewhere separate they can come to it when they're ready. What do
you think?-->
<!-- Ok, I can see that. /Carol -->

## Summary

You’re already off to a great start on your Rust journey! In this chapter,
you’ve:

* Installed the latest stable version of Rust
* Written a “Hello, world!” program using both `rustc` directly and using
  the conventions of `cargo`

This is a great time to build a more substantial program, to get used to
reading and writing Rust code. In the next chapter, we’ll build a guessing game
program. If you’d rather start by learning about how common programming
concepts work in Rust, see Chapter 3.
