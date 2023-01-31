# Програмування гри - відгадайки

Розпочнемо вивчення Rust зі спільної розробки проєкту! Цей розділ ознайомить вас із кількома поширеними концепціями Rust, демонструючи як вони використовуються у реальній програмі. You’ll learn about `let`, `match`, methods, associated functions, external crates, and more! In the following chapters, we’ll explore these ideas in more detail. In this chapter, you’ll just practice the fundamentals.

Ми розв'язуватимемо класичну задачу для програмістів-початківців: гру "відгадай число". Умови такі: програма генерує випадкове ціле число між 1 та 100. Потім пропонує гравцю ввести спробу відгадати. Після введення спроби вона скаже, чи число більше або менше за загадане. Якщо відгадано правильно, гра виведе привітання і припинить роботу.

## Початок нового проєкту

To set up a new project, go to the *projects* directory that you created in Chapter 1 and make a new project using Cargo, like so:

```console
$ cargo new guessing_game
$ cd guessing_game
```

Перша команда, `cargo new`, приймає першим параметром ім'я проєкту (`guessing_game`). Друга команда переходить до теки нового проєкту.

Перегляньмо щойно створений файл *Cargo.toml*:

<!-- manual-regeneration
cd listings/ch02-guessing-game-tutorial
rm -rf no-listing-01-cargo-new
cargo new no-listing-01-cargo-new --name guessing_game
cd no-listing-01-cargo-new
cargo run > output.txt 2>&1
cd ../../..
-->

<span class="filename">Файл: Cargo.toml</span>

```toml
{{#include ../listings/ch02-guessing-game-tutorial/no-listing-01-cargo-new/Cargo.toml}}
```

Як ви вже бачили у Розділі 1, `cargo new` створює програму "Hello, world!". Подивімося, що міститься у файлі *src/main.rs*:

<span class="filename">Файл: src/main.rs</span>

```rust
{{#rustdoc_include ../listings/ch02-guessing-game-tutorial/no-listing-01-cargo-new/src/main.rs}}
```

Now let’s compile this “Hello, world!” program and run it in the same step using the `cargo run` command:

```console
{{#include ../listings/ch02-guessing-game-tutorial/no-listing-01-cargo-new/output.txt}}
```

The `run` command comes in handy when you need to rapidly iterate on a project, as we’ll do in this game, quickly testing each iteration before moving on to the next one.

Відкрийте файл *src/main.rs*. Увесь код ми писатимемо у цьому файлі.

## Обробляємо здогадку

Перша частина програми буде просити у користувача ввести здогадку, обробляти те, що він увів, і перевіряти, чи ввів він дані у потрібній формі. Для початку, дозволимо користувачеві ввести здогадку. Введіть код з Блоку коду 2-1 до *src/main.rs*.

<span class="filename">Файл: src/main.rs</span>

```rust,ignore
{{#rustdoc_include ../listings/ch02-guessing-game-tutorial/listing-02-01/src/main.rs:all}}
```


<span class="caption">Listing 2-1: Code that gets a guess from the user and prints it</span>

Цей код містить багато інформації, тому розбиратимемо його рядок за рядком. Щоб отримати, що ввів користувач, і вивести результат, нам треба ввести бібліотеку введення/виведення `io` в область видимості. The `io` library comes from the standard library, known as `std`:

```rust,ignore
{{#rustdoc_include ../listings/ch02-guessing-game-tutorial/listing-02-01/src/main.rs:io}}
```

By default, Rust has a set of items defined in the standard library that it brings into the scope of every program. This set is called the *prelude*, and you can see everything in it [in the standard library documentation][prelude].

Якщо типу, який ви хочете використати, нема у прелюдії, вам доведеться явно вносити цей тип у область видимості за допомогою інструкції `use`. Використання бібліотеки `std::io` надає вам ряд корисних особливостей, включно з можливістю користувацького вводу.

As you saw in Chapter 1, the `main` function is the entry point into the program:

```rust,ignore
{{#rustdoc_include ../listings/ch02-guessing-game-tutorial/listing-02-01/src/main.rs:main}}
```

The `fn` syntax declares a new function; the parentheses, `()`, indicate there are no parameters; and the curly bracket, `{`, starts the body of the function.

As you also learned in Chapter 1, `println!` is a macro that prints a string to the screen:

```rust,ignore
{{#rustdoc_include ../listings/ch02-guessing-game-tutorial/listing-02-01/src/main.rs:print}}
```

This code is printing a prompt stating what the game is and requesting input from the user.

### Зберігання значень у змінних

Тепер створімо *змінну* для зберігання того, що користувач увів, ось так:

```rust,ignore
{{#rustdoc_include ../listings/ch02-guessing-game-tutorial/listing-02-01/src/main.rs:string}}
```

Тепер програма стає цікавішою! В цьому коротенькому рядку відбувається багато всього. Ми використовуємо інструкцію `let`, щоб створити змінну. Ось інший приклад:

```rust,ignore
let apples = 5;
```

Цей рядок створює нову змінну з назвою `apples` і зв'язує її зі значенням 5. In Rust, variables are immutable by default, meaning once we give the variable a value, the value won’t change. Детально ця концепція обговорюється в підрозділі ["Змінні та мутабельність"][variables-and-mutability]<!-- ignore -->
Розділу 3. Щоб зробити змінну мутабельною, слід додати `mut` перед її іменем:

```rust,ignore
let apples = 5; // немутабельна
let mut bananas = 5; // мутабельна
```

> Примітка: синтаксична конструкція `//` починає коментар, що продовжується до кінця рядка. Rust ігнорує весь вміст коментаря. Про коментарі детальніше йдеться в [Розділі 3][comments]<!-- ignore -->.

Повернімося до нашої ігрової програми - відгадайки. Тепер ви знаєте, що `let 
mut guess` створить мутабельну змінну на ім'я `guess`. Знак рівності (`=`) каже Rust, що тепер ми хочемо зв'язати щось зі змінною. On the right of the equal sign is the value that `guess` is bound to, which is the result of calling `String::new`, a function that returns a new instance of a `String`. [`String`][string]<!-- ignore --> `String` - це тип стрічки, що надається стандартною бібліотекою; це кодовані в UTF-8 шматки тексту, які можна нарощувати.

Синаксична конструкція `::` в рядку `` ::new` ``позначає, що `new` - це асоційована функція типу `String`. *Асоційована функція* є реалізованою для типу, в цьому випадку `String`. Ця функція `new` створює нову, порожню `String`. You’ll find a `new` function on many types because it’s a common name for a function that makes a new value of some kind.

В цілому: рядок `let mut guess = String::new();` створив мутабельну змінну, що зараз зв'язана з новим, порожнім екземпляром `String`. Хух!

### Отримання введення від користувача

Згадаймо, що ми додали функціональність введення/виведення зі стандартної бібліотеки за допомогою `use std::io;` у першому рядку програми. Тепер викличмо функцію `stdin` з модуля `io`, що дозволить обробляти те, що вводить користувач:

```rust,ignore
{{#rustdoc_include ../listings/ch02-guessing-game-tutorial/listing-02-01/src/main.rs:read}}
```

If we hadn’t imported the `io` library with `use std::io;` at the beginning of the program, we could still use the function by writing this function call as `std::io::stdin`. Функциія `stdin` повертає екземпляр [`std::io::Stdin`][iostdin]<!-- ignore -->; цей тип являє собою дескриптор (handle) стандартного потоку введення термінала.

Далі рядок `.read_line(&mut guess)` викликає метод [`read_line`][read_line]<!--
ignore --> дескриптора стандартного введення, щоб отримати, що ввів користувач. Ми також передаємо 

`&mut guess` аргументом до `read_line`, щоб повідомити йому, до якої стрічки зберегти введення користувача. Повне завдання `read_line` - взяти те, що користувач набрав у стандартний потік введення і додати до стрічки (не перезаписавши її вміст), тому ми передаємо стрічку як аргумент. Стрічка-аргумент має бути мутабельною, щоб метод міг змінити її вміст.

`&` позначає, що цей аргумент - *посилання*, що дає вам можливість надати кільком частинам вашого коду доступ до одного фрагменту даних без кількаразового копіювання цих даних у пам'яті. Посилання - складна тема, але одна з основних переваг Rust полягає в безпеці та легкості використання посилань. Для завершення цієї програми вам не знадобляться особливо детальні знання про посилання. For now, all you need to know is that, like variables, references are immutable by default. Тому необхідно писати`&mut guess`, а не просто`&guess`, щоб зробити його мутабельним. (Розділ 4 пояснить посилання ретельніше.)

<!-- Old heading. Do not remove or links may break. -->
<a id="handling-potential-failure-with-the-result-type"></a>

### Handling Potential Failure with `Result`

We’re still working on this line of code. We’re now discussing a third line of text, but note that it’s still part of a single logical line of code. The next part is this method:

```rust,ignore
{{#rustdoc_include ../listings/ch02-guessing-game-tutorial/listing-02-01/src/main.rs:expect}}
```

We could have written this code as:

```rust,ignore
io::stdin().read_line(&mut guess).expect("Failed to read line");
```

However, one long line is difficult to read, so it’s best to divide it. It’s often wise to introduce a newline and other whitespace to help break up long lines when you call a method with the `.method_name()` syntax. Now let’s discuss what this line does.

As mentioned earlier, `read_line` puts whatever the user enters into the string we pass to it, but it also returns a `Result` value. [`Result`][result]<!--
ignore --> is an 

[*enumeration*][enums]<!-- ignore -->, often called an *enum*, which is a type that can be in one of multiple possible states. We call each possible state a *variant*.

[Chapter 6][enums]<!-- ignore --> will cover enums in more detail. The purpose of these `Result` types is to encode error-handling information.

`Result`’s variants are `Ok` and `Err`. Варіант `Ok` показує, що операція була вдалою, і всередині варіанту `Ok` знаходиться успішно згенероване значення. Варіант `Err` позначає невдачу, і містить інформацію, як і чому операція була невдалою.

Значення типу `Result`, як і значення будь-якого іншого типу, мають визначені для них методи. Екземпляр `Result` має доступний для виклику [метод `expect`][expect]<!-- ignore -->
. Якщо цей екземпляр `Result` має значення `Err`, то `expect` викличе аварійне завершення програми та виведе повідомлення, яке ви передали до `expect` параметром. Якщо метод `read_line` поверне `Err`, це, швидше за все, станеться внаслідок помилки, яка станеться в операційній системі. Якщо цей екземпляр `Result` має значення `Ok`, `expect` візьме повернуте значення, яке знаходиться в `Ok`, і поверне тільки це значення, щоб ним можна було скористатися. В цьому випадку це значення - кількість байтів, введених користувачем до стандартного потоку.

Якщо ви не викличете `expect`, програма скомпілюється, проте ви отримаєте попередження:

```console
{{#include ../listings/ch02-guessing-game-tutorial/no-listing-02-without-expect/output.txt}}
```

Якщо ми не викличемо `expect`, програма скомпілюється, проте ми отримаємо попередження:

The right way to suppress the warning is to actually write error-handling code, but in our case we just want to crash this program when a problem occurs, so we can use `expect`. Ви дізнаєтеся про те, як відновити роботу програми при помилці, у [Розділі 9][recover]<!-- ignore -->.

### Вивід значень за допомогою заповнювачів `println!`

Якщо не враховувати завершувальної фігурної дужки, лишився лише один рядок, який ми ще не обговорили:

```rust,ignore
{{#rustdoc_include ../listings/ch02-guessing-game-tutorial/listing-02-01/src/main.rs:print_guess}}
```

Цей рядок виводить стрічку, в якій ми зберегли те, що ввів користувач. Фігурні дужки `{}` - це заповнювач: можна уявити, що `{}` - клешні маленького краба, що тримає значення на місці. When printing the value of a variable, the variable name can go inside the curly brackets. When printing the result of evaluating an expression, place empty curly brackets in the format string, then follow the format string with a comma-separated list of expressions to print in each empty curly bracket placeholder in the same order. Printing a variable and the result of an expression in one call to `println!` would look like this:

```rust
let x = 5;
let y = 10;

println!("x = {x} and y + 2 = {}", y + 2);
```

This code would print `x = 5 and y + 2 = 12`.

### Тестування першої частини

Протестуймо першу частину гри "відгадай число". Запустіть її за допомогою `cargo run`:

<!-- manual-regeneration
cd listings/ch02-guessing-game-tutorial/listing-02-01/
cargo clean
cargo run
input 6 -->

```console
$ cargo run
   Compiling guessing_game v0.1.0 (file:///projects/guessing_game)
    Finished dev [unoptimized + debuginfo] target(s) in 6.44s
     Running `target/debug/guessing_game`
Guess the number!
Please input your guess.
6
You guessed: 6
```

На цей момент перша частина гри завершена: ми отримуємо дані з клавіатури та виводимо їх.

## Генерація таємного числа

Тепер нам треба згенерувати таємне число, яке користувач пробуватиме відгадати. Таємне число має бути різним кожного разу, щоб у гру було цікаво грати більше одного разу. Використаймо випадкове число від 1 до 100, щоб гра була не надто складною. Rust поки що не має функціональності для генерації випадкових чисел у стандартній бібліотеці; натомість команда Rust надає [крейт `rand`][randcrate] з таким функціоналом.

### Генерація випадкового числа

Пам'ятайте, що крейт є набором файлів вихідного коду Rust. Проєкт, що ми збираємо - це *двійковий крейт*, який є виконуваним. The `rand` crate is a *library crate*, which contains code that is intended to be used in other programs and can’t be executed on its own.

Використання зовнішніх крейтів - найсильніший бік Cargo. Перед тим, як писати код, що використовує `rand`, ми маємо змінити файл *Cargo.toml*, додавши туди крейт `rand` як залежність. Open that file now and add the following line to the bottom, beneath the `[dependencies]` section header that Cargo created for you. Be sure to specify `rand` exactly as we have here, with this version number, or the code examples in this tutorial may not work:

<!-- When updating the version of `rand` used, also update the version of
`rand` used in these files so they all match:
* ch07-04-bringing-paths-into-scope-with-the-use-keyword.md
* ch14-03-cargo-workspaces.md
-->

<span class="filename">Файл: Cargo.toml</span>

```toml
{{#include ../listings/ch02-guessing-game-tutorial/listing-02-02/Cargo.toml:8:}}
```

У файлі *Cargo.toml* все, що йде після заголовку секції, належить до цієї секції - до початку нової секції. У секції `[dependencies]` ви повідомляєте Cargo, від яких зовнішніх крейтів залежить ваш проєкт і які версії цих крейтів вам потрібні. In this case, we specify the `rand` crate with the semantic version specifier `0.8.5`. Cargo розуміє [Семантичне версіювання][semver]<!-- ignore --> (яке іноді звуть *SemVer*), що є стандартом для запису номерів версій. The specifier `0.8.5` is actually shorthand for `^0.8.5`, which means any version that is at least 0.8.5 but below 0.9.0.

Cargo considers these versions to have public APIs compatible with version 0.8.5, and this specification ensures you’ll get the latest patch release that will still compile with the code in this chapter. Any version 0.9.0 or greater is not guaranteed to have the same API as what the following examples use.

Тепер, не змінюючи коду, побудуємо проєкт, як показано в Блоці коду 2-2.

<!-- manual-regeneration
cd listings/ch02-guessing-game-tutorial/listing-02-02/
rm Cargo.lock
cargo clean
cargo build -->

```console
$ cargo build
    Updating crates.io index
  Downloaded rand v0.8.5
  Downloaded libc v0.2.127
  Downloaded getrandom v0.2.7
  Downloaded cfg-if v1.0.0
  Downloaded ppv-lite86 v0.2.16
  Downloaded rand_chacha v0.3.1
  Downloaded rand_core v0.6.3
   Compiling libc v0.2.127
   Compiling getrandom v0.2.7
   Compiling cfg-if v1.0.0
   Compiling ppv-lite86 v0.2.16
   Compiling rand_core v0.6.3
   Compiling rand_chacha v0.3.1
   Compiling rand v0.8.5
   Compiling guessing_game v0.1.0 (file:///projects/guessing_game)
    Finished dev [unoptimized + debuginfo] target(s) in 2.53s
```


<span class="caption">Listing 2-2: The output from running `cargo build` after adding the rand crate as a dependency</span>

You may see different version numbers (but they will all be compatible with the code, thanks to SemVer!) and different lines (depending on the operating system), and the lines may be in a different order.

Тепер, коли ми маємо зовнішню залежність, Cargo витягає останні версії всього, що нам треба, з *реєстру*, тобто копії даних з [Crates.io][cratesio]. На crates.io в екосистемі Rust люди викладають свої проєкти Rust з відкритим кодом, щоб ними могли скористатися інші.

Після оновлення реєстру Cargo перевіряє секцію `[dependencies]` і завантажує крейти, вказані там, але яких у вас бракує. В цьому випадку, хоча ми вказали тільки залежність від `rand`, Cargo також завантажив інші крейти, від яких залежить робота `rand`. Після завантаження крейтів Rust їх компілює, а потім компілює проєкт із доступними залежностями.

Якщо ви знову запустите `cargo build`, не зробивши жодних змін, ви не отримаєте жодної відповіді окрім рядка `Finished`. Cargo знає, що він вже завантажив і скомпілював залежності, а ви не змінили нічого, що б їх стосувалося, у файлі *Cargo.toml*. Cargo також знає, що ви не змінили нічого у коді, тому він не буде його перекомпільовувати. Оскільки роботи у Cargo немає, він просто завершується.

If you open the *src/main.rs* file, make a trivial change, and then save it and build again, you’ll only see two lines of output:

<!-- manual-regeneration
cd listings/ch02-guessing-game-tutorial/listing-02-02/
touch src/main.rs
cargo build -->

```console
$ cargo build
   Compiling guessing_game v0.1.0 (file:///projects/guessing_game)
    Finished dev [unoptimized + debuginfo] target(s) in 2.53 secs
```

These lines show that Cargo only updates the build with your tiny change to the *src/main.rs* file. Залежності не змінилися, і Cargo знає, що може заново використати те, що він вже завантажив і скомпілював.

#### Файл *Cargo.lock* гарантує відтворюваність збірки

Cargo має механізм, що гарантує однаковість збірки того самого артефакту кожного разу, коли ви чи хтось інший збирає ваш код: Cargo використає тільки ті версії залежностей, які ви зазначили, доки ви не вкажете інші. For example, say that next week version 0.8.6 of the `rand` crate comes out, and that version contains an important bug fix, but it also contains a regression that will break your code. Щоб упоратися з цим, при першому запуску `cargo build` Rust створює файл *Cargo.lock*, що відтепер розміщується у теці *guessing_game*.

When you build a project for the first time, Cargo figures out all the versions of the dependencies that fit the criteria and then writes them to the *Cargo.lock* file. When you build your project in the future, Cargo will see that the *Cargo.lock* file exists and will use the versions specified there rather than doing all the work of figuring out versions again. Це дозволяє автоматично робити відтворювану збірку. In other words, your project will remain at 0.8.5 until you explicitly upgrade, thanks to the *Cargo.lock* file. Because the *Cargo.lock* file is important for reproducible builds, it’s often checked into source control with the rest of the code in your project.

#### Оновлення крейта для отримання нової версії

Коли ж ви *хочете* оновити крейт, Cargo надає іншу команду, `update`, яка ігнорує файл *Cargo.lock* і визначає всі останні версії, що відповідають специфікаціям у *Cargo.toml*. Cargo запише ці версії до файлу *Cargo.lock*. Otherwise, by default, Cargo will only look for versions greater than 0.8.5 and less than 0.9.0. If the `rand` crate has released the two new versions 0.8.6 and 0.9.0, you would see the following if you ran `cargo update`:

<!-- manual-regeneration
cd listings/ch02-guessing-game-tutorial/listing-02-02/
cargo update
assuming there is a new 0.8.x version of rand; otherwise use another update
as a guide to creating the hypothetical output shown here -->

```console
$ cargo update
    Updating crates.io index
    Updating rand v0.8.5 -> v0.8.6
```

Cargo ignores the 0.9.0 release. At this point, you would also notice a change in your *Cargo.lock* file noting that the version of the `rand` crate you are now using is 0.8.6. To use `rand` version 0.9.0 or any version in the 0.9.*x* series, you’d have to update the *Cargo.toml* file to look like this instead:

```toml
[dependencies]
rand = "0.9.0"
```

Наступного разу, коли ви запустите `cargo build`, Cargo оновить реєстр доступних крейтів і заново перечитає вимоги до `rand` відповідно до вказаної вами нової версії.

Можна ще багато розповісти про [Cargo][doccargo]<!-- ignore --> і [його екосистему][doccratesio]<!-- ignore -->, which we’ll discuss in Chapter 14, but for now, that’s all you need to know. Cargo робить використання бібліотек дуже простим, що дозволяє растацеанцям писати менші проєкти, зібрані з кількох пакетів.

### Генерація випадкового числа

Використаймо `rand` для генерації числа, що треба відгадати. Наступний крок - оновити *src/main.rs*, як показано в Блоці коду 2-3.

<span class="filename">Файл: src/main.rs</span>

```rust,ignore
{{#rustdoc_include ../listings/ch02-guessing-game-tutorial/listing-02-03/src/main.rs:all}}
```


<span class="caption">Listing 2-3: Adding code to generate a random number</span>

First we add the line `use rand::Rng;`. Трейт `Rng` визначає методи, які реалізує генератор випадкових чисел, і цей трейт має бути в області видимості, щоб ми могли скористатися цими методами. Розділ 10 розповість про трейти детальніше.

Далі ми додаємо всередині ще два рядки. In the first line, we call the `rand::thread_rng` function that gives us the particular random number generator we’re going to use: one that is local to the current thread of execution and is seeded by the operating system. Потім ми викликаємо метод генератора випадкових чисел `gen_range`. This method is defined by the `Rng` trait that we brought into scope with the `use rand::Rng;` statement. Метод `gen_range` приймає параметрами два числа і генерує випадкове число в діапазоні між ними. Вираз для діапазону, що ми його тут застосували, має форму `початок..=кінець` і включає нижню і верхню межі, тому треба вказувати `1..=100`, щоб отримати число між 1 та 100.

> Примітка: Ви, звісно, не можете одразу знати, які трейти використати і які методи та функції викликати з крейта, тому кожен крейт має документацію з інструкцією до використання. Another neat feature of Cargo is that running the `cargo doc
  --open` command will build documentation provided by all your dependencies locally and open it in your browser. Якщо вам цікавий інший функціонал, скажімо, крейту `rand`, запустіть `cargo doc --open` і клацніть `rand` на боковій панелі ліворуч.

Другий рядок, який ми додали до коду, виводить таємне число. Це корисно, поки ми розробляємо програму, щоб можна було перевірити її роботу, але ми видалимо його у фінальній версії. Буде не дуже цікаво, якщо програма виводитиме відповідь одразу по запуску!

Спробуємо запустити програму кілька разів:

<!-- manual-regeneration
cd listings/ch02-guessing-game-tutorial/listing-02-03/
cargo run
4
cargo run
5
-->

```console
$ cargo run
   Compiling guessing_game v0.1.0 (file:///projects/guessing_game)
    Finished dev [unoptimized + debuginfo] target(s) in 2.53s
     Running `target/debug/guessing_game`
Guess the number!
The secret number is: 7
Please input your guess.
4
You guessed: 4

$ cargo run
    Finished dev [unoptimized + debuginfo] target(s) in 0.02s
     Running `target/debug/guessing_game`
Guess the number!
The secret number is: 83
Please input your guess.
5
You guessed: 5
```

Ви маєте побачити різні випадкові числа, і вони мають бути між 1 та 100. Чудова робота!

## Порівняння здогадки з таємним числом

Тепер, коли ми маємо введене користувачем і випадкове числа, ми можемо їх порівняти. Цей крок показано в Блоці коду 2-4. Note that this code won’t compile just yet, as we will explain.

<span class="filename">Файл: src/main.rs</span>

```rust,ignore,does_not_compile
{{#rustdoc_include ../listings/ch02-guessing-game-tutorial/listing-02-04/src/main.rs:here}}
```


<span class="caption">Listing 2-4: Handling the possible return values of comparing two numbers</span>

Спершу ми додали ще одну інструкцію `use`, яка вводить тип `std::cmp::Ordering` зі стандартної бібліотеки в область видимості. Тип `Ordering` ("впорядкування") - це ще один енум, що має варіанти: `Less` ("менше"), `Greater` ("більше"), and `Equal` ("дорівнює"). Це три можливі результати порівняння двох значень.

Потім ми додали в кінець коду п'ять нових рядків, в яких використали тип `Ordering`. Метод `cmp` порівнює два значення і може бути викликаний для всього, що можна порівнювати. It takes a reference to whatever you want to compare with: here it’s comparing `guess` to `secret_number`. Потім він повертає варіант енуму `Ordering`, який ми внесли у область видимості за допомогою інструкції `use`. Ми скористалися виразом [`match`][match]<!-- ignore --> , щоб визначити, що робити далі залежно від варіанту `Ordering`, що його повернув виклик `cmp` зі значеннями `guess` та `secret_number`.

Вираз `match` складається з *рукавів*. Рукав складається зі *шаблона* (<0>pattern</0>) для порівняння та коду, який буде виконано, якщо значення, передане виразу `match`, відповідає шаблону цього рукава. Rust бере значення, передане `match`, і по черзі перевіряє шаблони рукавів. Patterns and the `match` construct are powerful Rust features: they let you express a variety of situations your code might encounter and they make sure you handle them all. Детально ці можливості будуть розглянуті в Розділах 6 і 18, відповідно.

Розберімо крок за кроком цей приклад з виразом `match`. Нехай користувач увів 50, а випадково згенероване цього разу таємне число -
38.

When the code compares 50 to 38, the `cmp` method will return `Ordering::Greater` because 50 is greater than 38. Вираз `match` отримує значення `Ordering::Greater` і починає перевіряти шаблони кожного рукава. Він перевіряє шаблон першого рукава, `Ordering::Less`, і бачить, що значення `Ordering::Greater` не відповідає `Ordering::Less`, тому пропускає рукав і переходить до наступного рукава. Шаблон наступного рукава, `Ordering::Greater`, *відповідає* `Ordering::Greater`! Код цього рукава буде виконано і виведе на екран `Too big!`. Вираз `match` завершується після першого вдалого порівняння, тому останній рукав в цьому випадку не буде перевірено.

Але Блок коду 2-4 все ще не компілюється. Спробуймо його скомпілювати:

<!--
The error numbers in this output should be that of the code **WITHOUT** the
anchor or snip comments
-->

```console
{{#include ../listings/ch02-guessing-game-tutorial/listing-02-04/output.txt}}
```

Суть цієї помилки в тому, що тут є *невідповідні типи*. Rust має сильну, статичну систему типів. Разом із тим, він має систему виведення типів. Коли ми писали `let mut guess = String::new()`, Rust зміг вивести, що `guess` має бути типу `String` і не просив нас написати тип. `secret_number`, з іншого боку, числового типу. Кілька числових типів Rust можуть мати значення між 1 та 100: `i32`, знакове 32-бітне число; `u32`, беззнакове 32-бітне число; `i64`, знакове 64-бітне число і кілька інших. Як не вказати іншого, Rust за замовчанням обере `i32`, і це й буде типом `secret_number`, якщо ви не додасте інформацію про тип деінде, щоб змусити Rust вивести інший числовий тип. Причина ж цієї помилки полягає в тому, що Rust не може порівнювати стрічку і числовий тип.

Зрештою, ми хочемо перетворити стрічку `String`, яку програма прочитала з клавіатури, в числовий тип, щоб можна було порівняти його як число зі таємним числом. We do so by adding this line to the `main` function body:

<span class="filename">Файл: src/main.rs</span>

```rust,ignore
{{#rustdoc_include ../listings/ch02-guessing-game-tutorial/no-listing-03-convert-string-to-number/src/main.rs:here}}
```

Ось цей рядок:

```rust,ignore
let guess: u32 = guess.trim().parse().expect("Please type a number!");
```

Ми створили змінну з назвою `guess`. Але чекайте, в програмі вже ніби існує змінна з назвою `guess`? It does, but helpfully Rust allows us to shadow the previous value of `guess` with a new one. *Shadowing* lets us reuse the `guess` variable name rather than forcing us to create two unique variables, such as `guess_str` and `guess`, for example. We’ll cover this in more detail in [Chapter 3][shadowing]<!-- ignore -->, but for now, know that this feature is often used when you want to convert a value from one type to another type.

Ми зв'язали нову змінну з виразом `guess.trim().parse()`. `guess` у цьому виразі стосується першої змінної `guess`, у якій міститься стрічка, введена користувачем. Метод `trim`, застосований до екземпляра `String`, видалить всі пробільні символи на початку і в кінці, що треба зробити, аби порівняти стрічку з `u32`, який містить виключно числові дані. Користувач має натиснути на <span class="keystroke">enter</span>, щоб спрацював метод `read_line` і данні були введені, але це додає символ нового рядка до стрічки. Наприклад, якщо користувач набере <span class="keystroke">5</span> і натисне <span
class="keystroke">enter</span>, `guess` буде виглядати як `5\n`. The `\n` represents “newline.” (On Windows, pressing <span
class="keystroke">enter</span> results in a carriage return and a newline, `\r\n`.) Метод `trim` видалить `\n` чи `\r\n`, і залишиться просто `5`.

The [`parse` method on strings][parse]<!-- ignore --> перетворює стрічку на інший тип. Тут ми застосовуємо його для перетворення стрічки в число. Ми маємо повідомити Rust, який саме числовий тип нам потрібен, за допомогою `let guess: u32`. Двокрапка (`:`) після `guess` каже Rust, що ми анотуємо тип змінної. У Rust є кілька вбудованих числових типів; `u32`, що ви бачите тут є беззнаковим 32-бітним цілим. Це непоганий вибір для невеликих додатних чисел. You’ll learn about other number types in [Chapter 3][integers]<!-- ignore -->.

Additionally, the `u32` annotation in this example program and the comparison with `secret_number` means Rust will infer that `secret_number` should be a `u32` as well. So now the comparison will be between two values of the same type!

Метод `parse` буде працювати тільки з символами, які можна логічно перетворити на числа, і тому легко може викликати помилки. Якщо, наприклад, стрічка містить `A👍%`, її неможливо буде перетворити на число. Because it might fail, the `parse` method returns a `Result` type, much as the `read_line` method does (discussed earlier in [“Handling Potential Failure with `Result`”](#handling-potential-failure-with-result)<!-- ignore-->). We’ll treat this `Result` the same way by using the `expect` method again. If `parse` returns an `Err` `Result` variant because it couldn’t create a number from the string, the `expect` call will crash the game and print the message we give it. If `parse` can successfully convert the string to a number, it will return the `Ok` variant of `Result`, and `expect` will return the number that we want from the `Ok` value.

Let’s run the program now:

<!-- manual-regeneration
cd listings/ch02-guessing-game-tutorial/no-listing-03-convert-string-to-number/
cargo run
  76
-->

```console
$ cargo run
   Compiling guessing_game v0.1.0 (file:///projects/guessing_game)
    Finished dev [unoptimized + debuginfo] target(s) in 0.43s
     Running `target/debug/guessing_game`
Guess the number!
The secret number is: 58
Please input your guess.
  76
You guessed: 76
Too big!
```

Чудово! Хоча ми й додали пробіли перед здогадкою, програма все одно зрозуміла, що користувач увів 76. Запустіть програму кілька разів, щоб перевірити різну поведінку на різних введених даних: введіть таємне число, більше за нього і менше.

Гра тепер майже працює, але користувачеві надається тільки одна можливість вгадати. Змінімо це, додавши цикл!

## Введення кількох здогадок за допомогою циклу

Ключове слово `loop` створює нескінчений цикл. Ми додамо цикл, щоб дати користувачам більше можливостей відгадати число:

<span class="filename">Файл: src/main.rs</span>

```rust,ignore
{{#rustdoc_include ../listings/ch02-guessing-game-tutorial/no-listing-04-looping/src/main.rs:here}}
```

Як ви можете бачити, ми перенесли в цикл усе від запрошення ввести здогадку і до кінця. Обов'язково додайте в ці рядки відступи у чотири пробілами та знову запустіть програму. Програма запрошує ввести нову здогадку до нескінченості, що, власне, є новою проблемою. Не схоже, що користувач може вийти!

Користувач завжди може перервати програму, натиснувши клавіатурне скорочення <span class="keystroke">ctrl-c</span>. Але є інший спосіб втекти від цього ненажерного чудовиська - згаданий при обговоренні `parse` у підрозділі ["Порівняння здогадки з таємним числом”](#comparing-the-guess-to-the-secret-number)<!--
ignore -->: якщо користувач введе щось, крім числа, програма аварійно завершиться. Ми можемо скористатися з цього, щоб користувач зумів вийти з програми, як показано тут:

<!-- manual-regeneration
cd listings/ch02-guessing-game-tutorial/no-listing-04-looping/
cargo run
(too small guess)
(too big guess)
(correct guess)
quit
-->

```console
$ cargo run
   Compiling guessing_game v0.1.0 (file:///projects/guessing_game)
    Finished dev [unoptimized + debuginfo] target(s) in 1.50s
     Running `target/debug/guessing_game`
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
thread 'main' panicked at 'Please type a number!: ParseIntError { kind: InvalidDigit }', src/main.rs:28:47
note: run with `RUST_BACKTRACE=1` environment variable to display a backtrace
```

Typing `quit` will quit the game, but as you’ll notice, so will entering any other non-number input. This is suboptimal, to say the least; we want the game to also stop when the correct number is guessed.

### Вихід після вдалої здогадки

Запрограмуймо гру виходити, якщо користувач виграв, додавши інструкцію `break`:

<span class="filename">Файл: src/main.rs</span>

```rust,ignore
{{#rustdoc_include ../listings/ch02-guessing-game-tutorial/no-listing-05-quitting/src/main.rs:here}}
```

Додавання рядку `break` після`You win!` примусить програму вийти з циклу, якщо користувач відгадав таємне число. Вихід із циклу призведе до виходу з програми, бо цикл - це остання частина функції `main`.

### Обробка неправильного введення

Для покращення роботи гри, замість аварійного виходу, коли користувач вводить не число, зробімо так, що гра ігнорувала те, що ввели, щоб користувач міг продовжувати відгадувати. Ми можемо зробити це, змінивши рядок, де `guess` перетворюється зі `String` на `u32`, як показано в Блоці коду 2-5.

<span class="filename">Файл: src/main.rs</span>

```rust,ignore
{{#rustdoc_include ../listings/ch02-guessing-game-tutorial/listing-02-05/src/main.rs:here}}
```


<span class="caption">Listing 2-5: Ignoring a non-number guess and asking for another guess instead of crashing the program</span>

Ми замінили виклик `expect` на вираз `match`, щоб перейти від аварійного завершення програми до обробки помилки. Згадаймо, що метод `parse` повертає тип `Result`, а `Result` - це енум, що має варіанти `Ok` та `Err`. Ми використовуємо тут вираз `match`, так само як робили з `Ordering`, що його повертає метод `cmp`.

If `parse` is able to successfully turn the string into a number, it will return an `Ok` value that contains the resultant number. Це значення `Ok` буде відповідати зразку першого рукава, і весь вираз `match` поверне значення `num`, яке `parse` обчислив і поклав всередину значення `Ok`. Це число потрапить саме туди, куди нам треба - в нову змінну `guess`, яку ми створюємо.

Якщо `parse` *не* зможе перетворити стрічку на число, він поверне значення `Err`, що міститиме більше інформації про помилку. Значення `Err` не відповідає шаблону `Ok(num)` у першому рукаві `match`, але відповідає шаблону `Err(_)` у другому. Підкреслення `_` перехопить будь-яке значення; в цьому випадку, ми кажемо, що вираз має відповідати будь-якому `Err`, незалежно від інформації, що міститься у ньому. Тож програма виконає код другого рукава, `continue`, який каже програмі перейти на наступну ітерацію циклу `loop` і знову запитати наступну спробу. Таким чином, програма ігнорує всі помилки, які можуть зустрітися `parse`!

Нарешті все у нашій програмі має працювати як треба. Спробуймо запустити її:

<!-- manual-regeneration
cd listings/ch02-guessing-game-tutorial/listing-02-05/
cargo run
(too small guess)
(too big guess)
foo
(correct guess)
-->

```console
$ cargo run
   Compiling guessing_game v0.1.0 (file:///projects/guessing_game)
    Finished dev [unoptimized + debuginfo] target(s) in 4.45s
     Running `target/debug/guessing_game`
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

Блискуче! Лишилася тільки одна дрібна правка, і гра-відгадайка буде завершена. Згадаймо, що програма все ще виводить таємне число. Це було потрібно для тестування, але псує гру. Видалімо `println!`, який виводить таємне число. Блок коду 2-6 показує остаточний код.

<span class="filename">Файл: src/main.rs</span>

```rust,ignore
{{#rustdoc_include ../listings/ch02-guessing-game-tutorial/listing-02-06/src/main.rs}}
```

<span class="caption">Блок коду 2-6: Повний код гри "відгадай число!"</span>

Отже, ви зуміли вдало зібрати гру "відгадай число". Вітаємо!

## Підсумок

Цей проєкт був вступом до багатьох концепцій мови Rust через практику: `let`, `match`, функції, використання зовнішніх крейтів та інших. У кількох наступних розділах ми детальніше розберемо ці концепції. Розділ 3 розповідає про концепції, які є у більшості мов програмування, такі як змінні, типи даних, функції і показує, як ними користуватися в Rust. Розділ 4 досліджує володіння, концепцію мови Rust, що є найбільш відмінною від інших мов. Розділ 5 обговорює синтаксис структур і методів, а Розділ 6 детально розкриває, як працюють енуми.

[prelude]: ../std/prelude/index.html
[variables-and-mutability]: ch03-01-variables-and-mutability.html#variables-and-mutability
[comments]: ch03-04-comments.html
[string]: ../std/string/struct.String.html
[iostdin]: ../std/io/struct.Stdin.html
[read_line]: ../std/io/struct.Stdin.html#method.read_line
[result]: ../std/result/enum.Result.html
[enums]: ch06-00-enums.html
[enums]: ch06-00-enums.html
[expect]: ../std/result/enum.Result.html#method.expect
[recover]: ch09-02-recoverable-errors-with-result.html
[randcrate]: https://crates.io/crates/rand
[semver]: http://semver.org
[cratesio]: https://crates.io/
[doccargo]: http://doc.crates.io
[doccratesio]: http://doc.crates.io/crates-io.html
[match]: ch06-02-match.html
[shadowing]: ch03-01-variables-and-mutability.html#shadowing
[parse]: ../std/primitive.str.html#method.parse
[integers]: ch03-02-data-types.html#integer-types
