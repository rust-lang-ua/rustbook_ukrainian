## Hello, World!

Після встановлення Rust напишемо першу програму цією мовою. Давно стало 
традицією при вивченні нової мови програмування писати маленьку програму, що
виводить на екран текст  “Hello, world!”, і ми не будемо відступати від цієї 
традиції.

> Примітка: ця книжка передбачає базове знайомство із командним рядком. Rust
> як така не висуває особливих вимог до редакторів, інструментів і розміщення
> коду, тому якщо вам зручніше використовувати IDE замість командного рядка,
> можете користуватися вашим улюбленим IDE.

### Створення теки проекту

Для початку, створіть теку для розміщення вашого коду на Rust. Для Rust немає
значення, де розміщено ваш код, але в цій книжці ми рекомендуємо зробити теку 
*projects* (проекти) у вашій домашній теці і тримати всі проекти там. Запустіть 
термінал і введіть такі команди, щоб створити теку для проекту з цього розділу:

Linux і Mac:

```text
$ mkdir ~/projects
$ cd ~/projects
$ mkdir hello_world
$ cd hello_world
```

Windows:

```cmd
> mkdir %USERPROFILE%\projects
> cd %USERPROFILE%\projects
> mkdir hello_world
> cd hello_world
```

### Написання і запуск програми на Rust

Теперь створіть новий сирцевий файл і назвіть його *main.rs*. Файли на Rust 
завжди закінчуються розширенням *.rs*. Якщо у назві файлу використовується більш
ніж одне слово, для розділення використовується підкреслення. Наприклад, можна
назвати файл *hello_world.rs*, але не *helloworld.rs*.

Тепер відкрийте файл *main.rs*, який ви щойно створили, і наберіть такий код:

<span class="filename">Filename: main.rs</span>

```rust
fn main() {
    println!("Hello, world!");
}
```

Збережіть цей файл і поверніться до вікна терміналу. На Linux або Mac наберіть 
такі команди:

```text
$ rustc main.rs
$ ./main
Hello, world!
```

У Windows запустіть `.\main.exe` замість `./main`. Незалежно від вашої 
операційної системи, ви побачите у терміналі стрічку `Hello, world!`. Якщо 
побачили - вітаємо! Ви щойно написали програму на Rust і офіційно стали 
програмістом на Rust! Ласкаво просимо до спільноти!

### Анатомія програми на Rust

Тепер давайте детально пройдемося по тому, що щойно сталося у вашій програмі 
"Hello, world!". Ось перший шматок пазла:

```rust
fn main() {

}
```

Ці рядки на Rust визначають *функцію*. Функція `main` особлива: це перше, що 
запускається у кожній виконанній програмі на Rust. Перший рядок каже: "я 
проголошую функцію з назовою `main` без параметрів і яка нічого не повертає. 
Якби були параметри, їхні імена треба було розмістити між дужками `(` та `)`.

Також зверніть увагу, що тіло функції взято у фігурні дужки `{` та `}`. Rust 
вимагає таких дужок навколо тіл усіх функцій. Вважається хорошим стилем
розміщувати відкриваючу дужку на тому ж рядку, що й проголошення функції, з
відступом в один пробіл.

Всередині функції `main`:

```rust
    println!("Hello, world!");
```

Цей рядок виконує всю роботу в цій маленькій програмі: виводить текст на екран.
Тут треба зазначити ряд деталей. По-перше, у Rust прийнято робити відступи в 
чотири пробіли, а не табуляцію.

Друга важлива деталь - це `println!`. Це виклик *макросу* (*macro*), реалізації
метапрограмування в Rust. Якби ми викликали функцію, то це виглядало б як 
`println` (без `!`). Макроси в Rust детальніше обговорюються в Розділі 24, а 
поки що достатньо знати, що коли ви бачите `!`, це означає, що викликається 
макрос, а не звичайна функція.

Далі іде `"Hello, world!"`  - це *стрічка* (*string*). Ми передаємо цю стрічку 
аргументом до `println!`, який виводить стрічку на екран. Досить просто!

Рядок завершується крапкою із комою (`;`). Це означає, що цей вираз завершено, і
можна починати наступний. Більшість рядків в коді на Rust завершується `;`.

### Компіляція і запуск - окремі кроки

В підрозділі "Написання і запуск програми на Rust" ми показали вам, як запускати
щойно створену програму. Тепер розділимо цей процес на частини і дослідимо кожну 
з них окремо.

Перед запуском програми на Rust необхідно її скомпілювати. Можна скористатися 
компілятором Rust, набравши команду rustc і передавши їй ім'я сирцевого файлу:

```text
$ rustc main.rs
```

Якщо ви маєте досвід роботи з C чи C++, ви можете помітити, що це схоже на `gcc` 
чи `clang`. Після вдалої компіляції Rust створює двійковий виконанний файл, який
можна побачити на Linux чи Mac, ввівши команду `ls` у вашій оболонці:

```text
$ ls
main  main.rs
```

В Windows введіть `dir /B` (опція /B каже показувати імена тільки файлів):

```cmd
> dir /B
main.exe
main.rs
```

Ми бачиому, що в нас є два файли: сирцевий код, з розширенням *.rs*, та 
виконанний файл (*main.exe* у Windows, *main* деінде). Все, що лишається на цей
момент - запустити файл *main* чи *main.exe*:

```text
$ ./main  # чи .\main.exe у Windows
```

Якщо *main.rs* - це ваша програма "Hello, world!", вона виведе повідомлення 
`Hello, world!` у вашому терміналі.

Якщо ви переходите з інтерпретованої мови на кшталт Ruby, Python чи JavaScript,
вам може бути незвичним, що компіляція і виконання програми - окремі кроки. Rust
є *завчасно компільованою* мовою, тобто ви можете скомпілювати програму, 
передати її комусь іншому, і він зможе запустити її навіть якщо у нього не 
встановлено Rust. Якщо ви передаєте комусь файл `.rb`, `.py` чи `.js`, йому,
натомість, буде потрібна встановлена реалізація мови Ruby, Python чи Javascript,
відповідно, але вам потрібна тільки одна команда щоб скомпілювати та запустити
вашу програму. В
If you come from a dynamic language like Ruby, Python, or JavaScript, you may
not be used to compiling and running a program being separate steps. Rust is an
*ahead-of-time compiled* language, which means that you can compile a program,
give it to someone else, and they can run it even without having Rust
installed. If you give someone a `.rb`, `.py`, or `.js` file, on the other
hand, they need to have a Ruby, Python, or JavaScript implementation installed
(respectively), but you only need one command to both compile and run your
program. Всі переваги мови програмування мають свою ціну.

Проста компіляція за допомогою `rustc` годиться для простеньких програм, але зі
зростанням вашого проекту, вам захочеться мати можливість керувати всіма 
параметрами, що є у вашому проекті і легко ділитися кодом з іншими людьми та
проектами. Наступний крок - інструмент, що зветься Cargo, який допоможе вам 
писати програми на Rust для реального світу.

## Hello, Cargo!

Cargo is Rust’s build system and package manager, and Rustaceans use Cargo to
manage their Rust projects because it makes a lot of tasks easier. For example,
Cargo takes care of building your code, downloading the libraries your code
depends on, and building those libraries. We call libraries your code needs
*dependencies*.

The simplest Rust programs, like the one we've written so far, don’t have any
dependencies, so right now, you'd only be using the part of Cargo that can take
care of building your code. As you write more complex Rust programs, you’ll
want to add dependencies, and if you start off using Cargo, that will be a lot
easier to do.

As the vast, vast majority of Rust projects use Cargo, we will assume that
you’re using it for the rest of the book. Cargo comes installed with Rust
itself, if you used the official installers as covered in the Installation
chapter. If you installed Rust through some other means, you can check if you
have Cargo installed by typing the following into your terminal:

```text
$ cargo --version
```

If you see a version number, great! If you see an error like `command not
found`, then you should look at the documentation for your method of
installation to determine how to install Cargo separately.

### Creating a Project with Cargo

Let's create a new project using Cargo and look at how it differs from our
project in `hello_world`. Go back to your projects directory (or wherever you
decided to put your code):

Linux and Mac:

```text
$ cd ~/projects
```

Windows:

```cmd
> cd %USERPROFILE%\projects
```

And then on any operating system run:

```text
$ cargo new hello_cargo --bin
$ cd hello_cargo
```

We passed the `--bin` argument to `cargo new` because our goal is to make an
executable application, as opposed to a library. Executables are binary
executable files often called just *binaries*. We've given `hello_cargo`
as the name for our project, and Cargo creates its files in a directory
of the same name that we can then go into.

If we list the files in the *hello_cargo* directory, we can see that Cargo has
generated two files and one directory for us: a *Cargo.toml* and a *src*
directory with a *main.rs* file inside. It has also initialized a new git
repository in the *hello_cargo* directory for us, along with a *.gitignore*
file; you can change this to use a different version control system, or no
version control system, by using the `--vcs` flag.

Open up *Cargo.toml* in your text editor of choice. It should look something
like this:

<span class="filename">Filename: Cargo.toml</span>

```toml
[package]
name = "hello_cargo"
version = "0.1.0"
authors = ["Your Name <you@example.com>"]

[dependencies]
```

This file is in the [*TOML*][toml]<!-- ignore --> (Tom's Obvious, Minimal
Language) format. TOML is similar to INI but has some extra goodies and is used
as Cargo’s configuration format.

[toml]: https://github.com/toml-lang/toml

The first line, `[package]`, is a section heading that indicates that the
following statements are configuring a package. As we add more information to
this file, we’ll add other sections.

The next three lines set the three bits of configuration that Cargo needs to
see in order to know that it should compile your program: its name, what
version it is, and who wrote it. Cargo gets your name and email information
from your environment. If it’s not correct, go ahead and fix that and save the
file.

The last line, `[dependencies]`, is the start of a section for you to list any
*crates* (which is what we call packages of Rust code) that your project will
depend on so that Cargo knows to download and compile those too. We won't need
any other crates for this project, but we will in the guessing game tutorial in
the next chapter.

Now let's look at *src/main.rs*:

<span class="filename">Filename: src/main.rs</span>

```rust
fn main() {
    println!("Hello, world!");
}
```

Cargo has generated a "Hello World!" for you, just like the one we wrote
earlier! So that part is the same. The differences between our previous project
and the project generated by Cargo that we've seen so far are:

- Our code goes in the *src* directory
- The top level contains a *Cargo.toml* configuration file

Cargo expects your source files to live inside the *src* directory so that the
top-level project directory is just for READMEs, license information,
configuration files, and anything else not related to your code. In this way,
using Cargo helps you keep your projects nice and tidy. There's a place for
everything, and everything is in its place.

If you started a project that doesn't use Cargo, as we did with our project in
the *hello_world* directory, you can convert it to a project that does use
Cargo by moving your code into the *src* directory and creating an appropriate
*Cargo.toml*.

### Building and Running a Cargo Project

Now let's look at what's different about building and running your Hello World
program through Cargo! To do so, enter the following commands:

```text
$ cargo build
   Compiling hello_cargo v0.1.0 (file:///projects/hello_cargo)
```

This should have created an executable file in *target/debug/hello_cargo* (or
*target\debug\hello_cargo.exe* on Windows), which you can run with this command:

```text
$ ./target/debug/hello_cargo # or .\target\debug\hello_cargo.exe on Windows
Hello, world!
```

Bam! If all goes well, `Hello, world!` should print to the terminal once more.

Running `cargo build` for the first time also causes Cargo to create a new file
at the top level called *Cargo.lock*, which looks like this:

<span class="filename">Filename: Cargo.lock</span>

```toml
[root]
name = "hello_cargo"
version = "0.1.0"
```

Cargo uses the *Cargo.lock* to keep track of dependencies in your application.
This project doesn't have dependencies, so the file is a bit sparse.
Realistically, you won't ever need to touch this file yourself; just let Cargo
handle it.

We just built a project with `cargo build` and ran it with
`./target/debug/hello_cargo`, but we can also use `cargo run` to compile
and then run:

```text
$ cargo run
     Running `target/debug/hello_cargo`
Hello, world!
```

Notice that this time, we didn't see the output telling us that Cargo was
compiling `hello_cargo`. Cargo figured out that the files haven’t changed, so
it just ran the binary. If you had modified your source code, Cargo would have
rebuilt the project before running it, and you would have seen something like
this:

```text
$ cargo run
   Compiling hello_cargo v0.1.0 (file:///projects/hello_cargo)
     Running `target/debug/hello_cargo`
Hello, world!
```

So a few more differences we've now seen:

- Instead of using `rustc`, build a project using `cargo build` (or build and
  run it in one step with `cargo run`)
- Instead of the result of the build being put in the same directory as our
  code, Cargo will put it in the *target/debug* directory.

The other advantage of using Cargo is that the commands are the same no matter
what operating system you're on, so at this point we will no longer be
providing specific instructions for Linux and Mac versus Windows.

### Building for Release

When your project is finally ready for release, you can use `cargo build
--release` to compile your project with optimizations. This will create an
executable in *target/release* instead of *target/debug*. These optimizations
make your Rust code run faster, but turning them on makes your program take
longer to compile. This is why there are two different profiles: one for
development when you want to be able to rebuild quickly and often, and one for
building the final program you’ll give to a user that won't be rebuilt and
that we want to run as fast as possible. If you're benchmarking the running
time of your code, be sure to run `cargo build --release` and benchmark with
the executable in *target/release*.

### Cargo as Convention

With simple projects, Cargo doesn't provide a whole lot of value over just
using `rustc`, but it will prove its worth as you continue. With complex
projects composed of multiple crates, it’s much easier to let Cargo coordinate
the build. With Cargo, you can just run `cargo build`, and it should work the
right way. Even though this project is simple, it now uses much of the real
tooling you’ll use for the rest of your Rust career. In fact, you can get
started with virtually all Rust projects you want to work
on with the following commands:

```text
$ git clone someurl.com/someproject
$ cd someproject
$ cargo build
```

> Note: If you want to look at Cargo in more detail, check out the official
[Cargo guide], which covers all of its features.

[Cargo guide]: http://doc.crates.io/guide.html
