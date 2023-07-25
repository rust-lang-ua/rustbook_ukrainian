## Робочі Області Cargo

В розділі 12, ми зібрали пакет, який включав двійковий крейт та бібліотечний крейт. В міру розвитку вашого проекту, ви можете виявити, що бібліотечний крейт продовжує становитися більшим і вам хочеться розділити ваш пакет на декілька бібліотечних крейтів. Cargo пропонує функціонал названий *робочими областями*, який може допомогти в керуванні кількома пов'язаними пакетами, які розробляються в тандемі.

### Створення Робочої Області

*Робочий область* це набір пакетів, які мають спільний *Cargo.lock* та каталог для виводу. Створимо проєкт з використанням робочої області — ми будемо використовувати тривіальний код, щоб було легше сконцентруватися на структурі робочого простору. Існує безліч способів упорядкування робочої області, тож ми просто покажемо один з найпоширеніших способів. У нас буде робоча область, що містить двійковий файл і дві бібліотеки. Двійковий файл, який надасть основний функціонал, буде залежати від двох бібліотек. Одна бібліотека надаватиме функцію `add_one`, а друга функцію `add_two`. Ці три крейти будуть частиною одної робочої області. Ми почнемо зі створення нового каталогу для робочої області:

```console
$ mkdir add
$ cd add
```

Далі, в каталозі *add*, ми створимо файл *Cargo.toml* який налаштує всю робочу область. Цей файл не матиме секції `[package]`. Натомість він розпочнеться з секції `[workspace]`, яка дозволить нам додавати учасників до робочої області, вказавши шлях до пакета із нашим двійковим крейтов; у цьому випадку, цей шлях *adder*:

<span class="filename">Файл: Cargo.toml</span>

```toml
{{#include ../listings/ch14-more-about-cargo/no-listing-01-workspace-with-adder-crate/add/Cargo.toml}}
```

Далі, ми створимо двійковий крейт `adder` запустивши `cargo new` всередині каталогу *add*:


<!-- manual-regeneration
cd listings/ch14-more-about-cargo/output-only-01-adder-crate/add
rm -rf adder
cargo new adder
copy output below
-->

```console
$ cargo new adder
     Created binary (application) `adder` package
```

Наразі ми можемо зібрати робочу область запустивши `cargo build`. Файли в вашому каталозі *add* мають виглядати наступним чином:

```text
├── Cargo.lock
├── Cargo.toml
├── adder
│   ├── Cargo.toml
│   └── src
│       └── main.rs
└── target
```

Робоча область має один каталог *target* на верхньому рівні, де будуть розміщені скомпільовані артефакти; пакет `adder` не має власного каталогу *target*. Навіть якщо ми запустимо `cargo build` зсередини каталогу *adder*, всі скомпільовані артефакти все одно з'являться в *add/target*, а не в *add/adder/target*. Cargo структурує каталог *target* в робочій області наступним чином, бо крейти в робочому просторі призначені для того, щоб залежати одне від одного. Якщо кожен крейт мав би власний каталог *target*, то кожен крейт мав би повторно компілювати кожен інший крейт в робочій області, щоб розмістити артефакти в власному каталозі *target*. При спільному використанні каталогу *target*, крейти можуть уникнути непотрібних повторних збірок.

### Створення Другого Пакета в Робочій Області

Далі створимо ще один (member) пакет в робочій області і назвемо його `add_one`. Змініть *Cargo.toml* верхнього рівня, вказав шлях до *add_one* в списку `members`:

<span class="filename">Файл: Cargo.toml</span>

```toml
{{#include ../listings/ch14-more-about-cargo/no-listing-02-workspace-with-two-crates/add/Cargo.toml}}
```

Потім згенеруйте новий бібліотечний крейт, названий `add_one`:


<!-- manual-regeneration
cd listings/ch14-more-about-cargo/output-only-02-add-one/add
rm -rf add_one
cargo new add_one --lib
copy output below
-->

```console
$ cargo new add_one --lib
     Created library `add_one` package
```

Ваш каталог *add* тепер повинен мати ці каталоги та файли:

```text
├── Cargo.lock
├── Cargo.toml
├── add_one
│   ├── Cargo.toml
│   └── src
│       └── lib.rs
├── adder
│   ├── Cargo.toml
│   └── src
│       └── main.rs
└── target
```

У файлі *add_one/src/lib.rs*, додамо функцію `add_one`:

<span class="filename">Файл: add_one/src/lib.rs</span>

```rust,noplayground
{{#rustdoc_include ../listings/ch14-more-about-cargo/no-listing-02-workspace-with-two-crates/add/add_one/src/lib.rs}}
```

Тепер ми можемо мати пакет `adder` із нашим двійковим файлом, залежним від пакета `add_one`, який є в нашій бібліотеці. Спочатку нам потрібно додати шлях залежності `add_one` в *adder/Cargo.toml*.

<span class="filename">Файл: adder/Cargo.toml</span>

```toml
{{#include ../listings/ch14-more-about-cargo/no-listing-02-workspace-with-two-crates/add/adder/Cargo.toml:6:7}}
```

Cargo не припускає, що крейти в робочій області будуть залежати одне від одного, тому нам потрібно чітко визначити відносини залежностей.

Далі, використаємо функцію `add_one` (з крейту `add_one`) в крейті `adder`. Відкрийте файл *adder/src/main.rs* і додайте рядок `use` зверху, щоб внести новий бібліотечний крейт `add_one` в область видимості. Потім змініть функцію `main` та викличте функцію `add_one`, як показано в Блоці Коду 14-7.

<span class="filename">Файл: adder/src/main.rs</span>

```rust,ignore
{{#rustdoc_include ../listings/ch14-more-about-cargo/listing-14-07/add/adder/src/main.rs}}
```


<span class="caption">Блок Коду 14-7: Використання бібліотечного крейту `add_one` з крейту `adder`</span>

Зберемо робочу область викликавши `cargo build` на верхньому рівні каталогу *add*!


<!-- manual-regeneration
cd listings/ch14-more-about-cargo/listing-14-07/add
cargo build
copy output below; the output updating script doesn't handle subdirectories in paths properly
-->

```console
$ cargo build
   Compiling add_one v0.1.0 (file:///projects/add/add_one)
   Compiling adder v0.1.0 (file:///projects/add/adder)
    Finished dev [unoptimized + debuginfo] target(s) in 0.68s
```

Щоб запустити двійковий крейт із каталогу *add*, нам потрібно вказати, який пакет в робочій області ми хочемо запускати використовуючи аргумент `-p` та назвою пакета в `cargo run`:


<!-- manual-regeneration
cd listings/ch14-more-about-cargo/listing-14-07/add
cargo run -p adder
copy output below; the output updating script doesn't handle subdirectories in paths properly
-->

```console
$ cargo run -p adder
    Finished dev [unoptimized + debuginfo] target(s) in 0.0s
     Running `target/debug/adder`
Hello, world! 10 plus one is 11!
```

Цей код в *adder/src/main.rs*, що залежить від крейту `add_one`.

#### Залежність від Зовнішнього Пакета в Робочій Області

Зауважте, що робоча область має лише один файл *Cargo.lock* на верхньому рівні, замість того, щоб мати *Cargo.lock* в кожному каталозі крейту. Завдяки цьому всі крейти використовують однакову версію всіх залежностей. Якщо ми додамо пакет `rand` в файли *adder/Cargo.toml* та *add_one/Cargo.toml*, Cargo вирішить використовувати одну версію `rand` для обох та запише це в одному *Cargo.lock*. Використання одних і тих самих залежностей для всіх крейтів в одній робочій області означає, що крейти завжди будуть сумісні один з одним. Додамо крейт `rand` в секцію `[dependencies]` в файл *add_one/Cargo.toml*, щоб ми могли використовувати крейт `rand` в крейті `add_one`:


<!-- When updating the version of `rand` used, also update the version of
`rand` used in these files so they all match:
* ch02-00-guessing-game-tutorial.md
* ch07-04-bringing-paths-into-scope-with-the-use-keyword.md
-->

<span class="filename">Файл: add_one/Cargo.toml</span>

```toml
{{#include ../listings/ch14-more-about-cargo/no-listing-03-workspace-with-external-dependency/add/add_one/Cargo.toml:6:7}}
```

Тепер ми можемо додати `use rand;` в файл *add_one/src/lib.rs* і збірка цілої робочої області, запустивши `cargo build` в каталозі *add*, принесе та скомпілює крейт `rand`. Ми отримаємо одне попередження, бо ми не посилаємось на принесений `rand` в нашій області видимості:


<!-- manual-regeneration
cd listings/ch14-more-about-cargo/no-listing-03-workspace-with-external-dependency/add
cargo build
copy output below; the output updating script doesn't handle subdirectories in paths properly
-->

```console
$ cargo build
    Updating crates.io index
  Downloaded rand v0.8.5
   --snip--
   Compiling rand v0.8.5
   Compiling add_one v0.1.0 (file:///projects/add/add_one)
warning: unused import: `rand`
 --> add_one/src/lib.rs:1:5
  |
1 | use rand;
  |     ^^^^
  |
  = note: `#[warn(unused_imports)]` on by default

warning: `add_one` (lib) generated 1 warning
   Compiling adder v0.1.0 (file:///projects/add/adder)
    Finished dev [unoptimized + debuginfo] target(s) in 10.18s
```

*Cargo.lock* на верхньому рівні тепер містить інформацію про залежність `add_one` від `rand`. Однак, навіть якщо `rand` використовується десь в робочій області, ми не можемо використовувати його в інших крейтах в робочій області, якщо також не додамо `rand` до їхніх файлів *Cargo.toml*. Наприклад, якщо ми додамо `use rand;` в файл *adder/src/main.rs* пакету `adder`, ми отримаємо помилку:


<!-- manual-regeneration
cd listings/ch14-more-about-cargo/output-only-03-use-rand/add
cargo build
copy output below; the output updating script doesn't handle subdirectories in paths properly
-->

```console
$ cargo build
  --snip--
   Compiling adder v0.1.0 (file:///projects/add/adder)
error[E0432]: unresolved import `rand`
 --> adder/src/main.rs:2:5
  |
2 | use rand;
  |     ^^^^ no external crate `rand`
```

Щоб це виправити, відредагуйте файл *Cargo.toml* пакету `adder` і вкажіть, що `rand` і для нього є залежністю. Збірка пакету `adder` додасть `rand` в список залежностей `adder` в *Cargo.lock*, але жодних додаткових копій `rand` не буде завантажено. Cargo має гарантувати, що кожен крейт у кожному пакеті в робочій області використовує пакет `rand` однакової версії, що збереже нам простір та запевнить, що крейти в робочій області будуть сумісними один з одним.

#### Додавання Тесту до Робочої Області

Для ще одного покращення, додамо тест функції `add_one::add_one` всередині крейту `add_one`:

<span class="filename">Файл: add_one/src/lib.rs</span>

```rust,noplayground
{{#rustdoc_include ../listings/ch14-more-about-cargo/no-listing-04-workspace-with-tests/add/add_one/src/lib.rs}}
```

Тепер запустіть `cargo test` в найвищому рівні каталогу *add*. Запуск `cargo test` в робочій області із подібною до цієї структурою буде запускати тести усіх крейтів цієї робочої області:


<!-- manual-regeneration
cd listings/ch14-more-about-cargo/no-listing-04-workspace-with-tests/add
cargo test
copy output below; the output updating script doesn't handle subdirectories in
paths properly
-->

```console
$ cargo test
   Compiling add_one v0.1.0 (file:///projects/add/add_one)
   Compiling adder v0.1.0 (file:///projects/add/adder)
    Finished test [unoptimized + debuginfo] target(s) in 0.27s
     Running unittests src/lib.rs (target/debug/deps/add_one-f0253159197f7841)

running 1 test
test tests::it_works ... ok

test result: ok. 1 passed; 0 failed; 0 ignored; 0 measured; 0 filtered out; finished in 0.00s

     Running unittests src/main.rs (target/debug/deps/adder-49979ff40686fa8e)

running 0 tests

test result: ok. 0 passed; 0 failed; 0 ignored; 0 measured; 0 filtered out; finished in 0.00s

   Doc-tests add_one

running 0 tests

test result: ok. 0 passed; 0 failed; 0 ignored; 0 measured; 0 filtered out; finished in 0.00s
```

Перша секція виводу показує, що тест `it_works` крейту `add_one` проходить. Наступна секція показує, що нуль тестів було знайдено в крейті `adder`, і потім, остання секція показує нуль документаційних тестів в крейті `add_one`.

We can also run tests for one particular crate in a workspace from the top-level directory by using the `-p` flag and specifying the name of the crate we want to test:


<!-- manual-regeneration
cd listings/ch14-more-about-cargo/no-listing-04-workspace-with-tests/add
cargo test -p add_one
copy output below; the output updating script doesn't handle subdirectories in paths properly
-->

```console
$ cargo test -p add_one
    Finished test [unoptimized + debuginfo] target(s) in 0.00s
     Running unittests src/lib.rs (target/debug/deps/add_one-b3235fea9a156f74)

running 1 test
test tests::it_works ... ok

test result: ok. 1 passed; 0 failed; 0 ignored; 0 measured; 0 filtered out; finished in 0.00s

   Doc-tests add_one

running 0 tests

test result: ok. 0 passed; 0 failed; 0 ignored; 0 measured; 0 filtered out; finished in 0.00s
```

Цей вивід показує, що `cargo test` запустив лише тести за крейту `add_one` та не запускав тести крейту `adder`.

Якщо ви опублікували крейти в робочій області до [crates.io](https://crates.io/), то кожен крейт в робочій області потрібно буде публікувати окремо. Як із `cargo test`, ми можемо публікувати певний крейт із нашої робочої області використовуючи позначку `-p` та вказуючи назву крейту, який ми хочемо опублікувати.

Для додаткової практики, додайте крейт `add_two` до цієї робочої області схожим чином до крейту `add_one`!

У міру зростання вашого проєкту, розгляньте можливість використання робочої області: легше зрозуміти менші, окремі компоненти ніж один великий блоб коду. Щобільше, зберігаючи крейти в робочій області можна зробити координацію між крейтами легшою, якщо вони часто та одночасно змінюються.
