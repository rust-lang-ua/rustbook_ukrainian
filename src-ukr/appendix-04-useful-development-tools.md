## Додаток D - корисні інструменти розробки

В цьому додатку ми говоримо про деякі корисні інструменти розробки, які надає проєкт Rust. Ми оглянемо автоматичне форматування, швидкі способи застосувати виправлення для попереджень, linter і інтеграцію з IDE.

### Автоматичне форматування за допомогою `rustfmt`

Інструмент `rustfmt` переформатовує ваш код відповідно до стилю коду спільноти. Багато спільних проєктів використовують `rustfmt` для запобігання суперечкам, який стиль використовувати під час написання Rust: всі форматують свій код за допомогою цього інструменту.

Щоб встановити `rustfmt`, введіть наступне:

```console
$ rustup component add rustfmt
```

Ця команда дає вам `rustfmt` і `cargo-fmt`, подібно до того, як Rust дає вам `rustc` та `cargo`. Щоб відформатувати будь-який проєкт Cargo, введіть наступне:

```console
$ cargo fmt
```

Запуск цієї команди переформатує весь код Rust в поточному крейті. Це має змінювати лише стиль коду, а не його семантику. Для отримання додаткової інформації по `rustfmt` перегляньте [його документацію][rustfmt].

### Виправте ваш код за допомогою `rustfix`

Інструмент rustfix включений у встановлення Rust і може автоматично виправити попередження компілятора, які мають чіткий спосіб виправити проблему, що скоріш за все те, що ви хочете. Ймовірно, ви вже бачили попередження компілятора. Наприклад, розглянемо цей код:

<span class="filename">Файл: src/main.rs</span>

```rust
fn do_something() {}

fn main() {
    for i in 0..100 {
        do_something();
    }
}
```

Тут ми викликаємо функцію `do_something` 100 разів. але ми ніколи не використовуємо змінну `і` в тілі циклу `for`. Rust попереджає нас про це:

```console
$ cargo build
   Compiling myprogram v0.1.0 (file:///projects/myprogram)
warning: unused variable: `i`
 --> src/main.rs:4:9
  |
4 |     for i in 0..100 {
  |         ^ help: consider using `_i` instead
  |
  = note: #[warn(unused_variables)] on by default

    Finished dev [unoptimized + debuginfo] target(s) in 0.50s
```

Попередження пропонує нам використати `_i` як назву змінної: підкреслення вказує на те, що не збираємося використовувати цю змінну. Ми можемо автоматично застосувати цю пропозицію, використовуючи інструмент `rustfix`, запустивши команду `cargo
fix`:

```console
$ cargo fix
    Checking myprogram v0.1.0 (file:///projects/myprogram)
      Fixing src/main.rs (1 fix)
    Finished dev [unoptimized + debuginfo] target(s) in 0.59s
```

Коли ми знову подивимось на *src/main.rs*, то побачимо, що `cargo fix` змінив код:

<span class="filename">Файл: src/main.rs</span>

```rust
fn do_something() {}

fn main() {
    for _i in 0..100 {
        do_something();
    }
}
```

Змінна циклу `for` тепер називається `_i`, і попередження більше не з'являється.

Ви також можете використовувати команду `cargo fix` для перенесення коду між різними редакціями Rust. Про редакції розповідає Додаток E.

### Більше lint від Clippy

Clippy - це інструмент, що містить набір lint для аналізу вашого коду, щоб ви могли спіймати загальні помилки та поліпшити ваш код на Rust.

Щоб встановити Clippy, введіть наступне:

```console
$ rustup component add clippy
```

Щоб запустити lint Clippy для будь-якого проєкту Cargo, введіть наступне:

```console
$ cargo clippy
```

Наприклад, ви пишете програму, що використовує наближення математичної константи, такої як Пі, як ця програма:

<span class="filename">Файл: src/main.rs</span>

```rust
fn main() {
    let x = 3.1415;
    let r = 8.0;
    println!("the area of the circle is {}", x * r * r);
}
```

Запуск `cargo clippy` на цьому проєкті призводить до помилки:

```text
error: approximate value of `f{32, 64}::consts::PI` found. Consider using it directly
 --> src/main.rs:2:13
  |
2 |     let x = 3.1415;
  |             ^^^^^^
  |
  = note: #[deny(clippy::approx_constant)] on by default
  = help: for further information visit https://rust-lang-nursery.github.io/rust-clippy/master/index.html#approx_constant
```

Ця помилка повідомляє, що Rust вже має більш точну константу `Пі` і що ваша програма буде коректнішою, якщо ви скористаєтеся цією константою. Тоді ви зміните свій код, щоб використовувати константу `Пі`. Наступний код не призводить до помилок або попереджень від Clippy:

<span class="filename">Файл: src/main.rs</span>

```rust
fn main() {
    let x = std::f64::consts::PI;
    let r = 8.0;
    println!("the area of the circle is {}", x * r * r);
}
```

Для отримання додаткової інформації про Clippy перегляньте [його документацію][clippy].

### Інтеграція в IDE за допомогою `rust-analyzer`

Для покращення інтеграції в IDE спільнота Rust рекомендує використовувати [`rust-analyzer`][rust-analyzer]<!-- ignore -->. Цей інструмент є набором довколокомпіляторних утиліт, що спілкуються за допомогою [Language Server Protocol][lsp]<!--
ignore -->, що є специфікацією для IDE і мов програмування для взаємного спілкування. Різні клієнти можуть використовувати 

`rust-analyzer`, наприклад [the Rust analyzer plug-in for Visual Studio Code][vscode].

Відвідайте [домашню сторінку][rust-analyzer] проєкту `rust-analyzer`, щоб отримати інструкцію для встановлення, тоді встановіть підтримку мовного сервера у вашому конкретному IDE. Ваше IDE набуде можливостей, таких, як автодоповнення, перехід до визначення і вбудовані помилки.

[rustfmt]: https://github.com/rust-lang/rustfmt

[clippy]: https://github.com/rust-lang/rust-clippy

[lsp]: http://langserver.org/
[vscode]: https://marketplace.visualstudio.com/items?itemName=rust-lang.rust-analyzer

[rust-analyzer]: https://rust-analyzer.github.io

[rust-analyzer]: https://rust-analyzer.github.io
