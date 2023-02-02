## Функції

Функції використовуються скрізь у коді на Rust. Ви вже бачили одну з найважливіших функцій у мові - функцію `main`, яка є точкою входу багатьох програм. Ви також бачили ключове слово `fn`, яке дозволяє вам оголошувати нові функції.

У мові Rust для назв функцій і змінних домовлено використовувати *зміїний регістр* - тобто всі літери маленькі, а слова відокремлюються підкресленнями. Ось приклад програми, що містить визначення функції:

<span class="filename">Файл: src/main.rs</span>

```rust
{{#rustdoc_include ../listings/ch03-common-programming-concepts/no-listing-16-functions/src/main.rs}}
```

Визначення функцій у Rust починаються з `fn`, далі іде назва функції і пара дужок. Фігурні дужки кажуть компілятору, де починається і закінчується тіло функції.

Ми можемо викликати будь-яку визначену нами функцію, написавши її назву і пару дужок. Оскільки в програмі є визначеної `another_function`, її можна викликати зсередини функції `main`. Зверніть увагу, що ми визначили `another_function` у початковому коді *після* функції `main`; так само її можна було визначити до функції <0>main</0>. Для Rust не має значення, де ви визначаєте функції, важливо, щоб вони були визначені хоч десь у області видимості, доступної з місця виклику.

Почнімо новий двійковий проєкт з назвою *functions*, щоб глибше дослідити функції. Помістіть приклад `another_function` до файлу *src/main.rs* і запустіть його. Ви маєте побачити таке:

```console
{{#include ../listings/ch03-common-programming-concepts/no-listing-16-functions/output.txt}}
```

Рядки виконуються в порядку, в якому вони знаходяться в функції `main`. First the “Hello, world!” message prints, and then `another_function` is called and its message is printed.

### Параметри

При визначенні функції ми можемо задати *параметри*, тобто спеціальні змінні, що є частиною сигнатури функції. Коли функція має параметри, ми можемо надати функції конкретні значення для цих параметрів. Формально, конкретні значення звуться *аргументами* або *фактичними параметрами*, а параметри у визначенні функції - *формальними параметрами*, але зазвичай слова <0>параметр</0> та <0>аргумент</0> використовуються як для частини визначення функції, так і для конкретних значень, які були передані при виклику функції.

Додамо параметр у нову версію `another_function`:

<span class="filename">Файл: src/main.rs</span>

```rust
{{#rustdoc_include ../listings/ch03-common-programming-concepts/no-listing-17-functions-with-parameters/src/main.rs}}
```

Запустіть цю програму; вона має вивести таке:

```console
{{#include ../listings/ch03-common-programming-concepts/no-listing-17-functions-with-parameters/output.txt}}
```

Проголошення `another_function` містить один параметр під назвою `x`. Тип `x` зазначено як `i32`. Коли в `another_function `передається `5`, макрос `println!` виведе `5` на місце фігурних дужок з `x` у форматній стрічці.

У сигнатурі функції ви *обов'язково* маєте проголошувати тип кожного параметру. Це свідоме рішення у дизайні мови Rust: обов'язкові анотації типів у визначенні функцій означають, що компілятору дуже рідко знадобиться просити вас використовувати їх деінде ще в коді, щоб зрозуміти, який тип ви мали на увазі. Також компілятор може надавати більш помічні повідомлення про помилки, якщо знатиме, які типи параметрів очікує функція.

When defining multiple parameters, separate the parameter declarations with commas, like this:

<span class="filename">Файл: src/main.rs</span>

```rust
{{#rustdoc_include ../listings/ch03-common-programming-concepts/no-listing-18-functions-with-multiple-parameters/src/main.rs}}
```

Цей приклад створює функцію `print_labeled_measurement` з двома параметрами. Перший параметр зветься `value` і має тип `i32`. Другий зветься `unit_label` і має тип `char`. Функція виводить текст, що містить і `value`, і `unit_label`.

Спробуймо запустити цей код. Замініть програму у файлі *src/main.rs* вашого проєкту *functions* попереднім прикладом, і запустіть його командою `cargo run`:

```console
{{#include ../listings/ch03-common-programming-concepts/no-listing-18-functions-with-multiple-parameters/output.txt}}
```

Because we called the function with `5` as the value for `value` and `'h'` as the value for `unit_label`, the program output contains those values.

### Тіла функцій

Тіла функцій складаються з послідовності інструкцій, яка може закінчуватися виразом. Поки що ми функції, які ми згадували, не мали виразу наприкінці, але вирази були частиною інструкцій. Оскільки Rust є мовою, що ґрунтується на виразах, важливо розуміти цю відмінність. Інші мови не мають таких відмінностей, тому роздивімося, що таке інструкції та вирази і як різниця між ними впливає на тіла функцій.

* **Statements** are instructions that perform some action and do not return a value.
* **Expressions** evaluate to a resultant value. Let’s look at some examples.

We’ve actually already used statements and expressions. Creating a variable and assigning a value to it with the `let` keyword is a statement. In Listing 3-1, `let y = 6;` is a statement.

<span class="filename">Файл: src/main.rs</span>

```rust
{{#rustdoc_include ../listings/ch03-common-programming-concepts/listing-03-01/src/main.rs}}
```

<span class="caption">Блок коду 3-1. Проголошення функції `main`, що містить одну інструкцію</span>

Function definitions are also statements; the entire preceding example is a statement in itself.

Statements do not return values. Therefore, you can’t assign a `let` statement to another variable, as the following code tries to do; you’ll get an error:

<span class="filename">Файл: src/main.rs</span>

```rust,ignore,does_not_compile
{{#rustdoc_include ../listings/ch03-common-programming-concepts/no-listing-19-statements-vs-expressions/src/main.rs}}
```

When you run this program, the error you’ll get looks like this:

```console
{{#include ../listings/ch03-common-programming-concepts/no-listing-19-statements-vs-expressions/output.txt}}
```

The `let y = 6` statement does not return a value, so there isn’t anything for `x` to bind to. This is different from what happens in other languages, such as C and Ruby, where the assignment returns the value of the assignment. In those languages, you can write `x = y = 6` and have both `x` and `y` have the value `6`; that is not the case in Rust.

Expressions evaluate to a value and make up most of the rest of the code that you’ll write in Rust. Consider a math operation, such as `5 + 6`, which is an expression that evaluates to the value `11`. Expressions can be part of statements: in Listing 3-1, the `6` in the statement `let y = 6;` is an expression that evaluates to the value `6`. Calling a function is an expression. Calling a macro is an expression. A new scope block created with curly brackets is an expression, for example:

<span class="filename">Файл: src/main.rs</span>

```rust
{{#rustdoc_include ../listings/ch03-common-programming-concepts/no-listing-20-blocks-are-expressions/src/main.rs}}
```

This expression:

```rust,ignore
{
    let x = 3;
    x + 1
}
```

is a block that, in this case, evaluates to `4`. That value gets bound to `y` as part of the `let` statement. Note that the `x + 1` line doesn’t have a semicolon at the end, which is unlike most of the lines you’ve seen so far. Вирази не мають завершувальної крапки з комою. Якщо ви додасте крапку з комою в кінець виразу, ви зробите його інструкцією, яка не повертає значення. Пам'ятайте це, коли вивчатимете далі значення, які повертають функції та вирази.

### Функції, що повертають значення

Функції можуть повертати значення в код, що їх викликав. Цим значенням ми не даємо власних імен, але маємо проголосити їхній тип після стрілочки (`->`). У Rust значення, що його повертає функція - це те саме, що значення останнього виразу в блоці - тілі функції. You can return early from a function by using the `return` keyword and specifying a value, but most functions return the last expression implicitly. Here’s an example of a function that returns a value:

<span class="filename">Файл: src/main.rs</span>

```rust
{{#rustdoc_include ../listings/ch03-common-programming-concepts/no-listing-21-function-return-values/src/main.rs}}
```

There are no function calls, macros, or even `let` statements in the `five` function—just the number `5` by itself. І це абсолютно коректна функція в Rust. Зверніть увагу, що тут зазначено тип значення, яке функція повертає `-> i32`. Спробуймо запустити цей код; вивід має виглядати так:

```console
{{#include ../listings/ch03-common-programming-concepts/no-listing-21-function-return-values/output.txt}}
```

`5` у `five` є значенням, яке повертає функція, і тому тип, який повертає функція - `i32`. Розгляньмо це детальніше. Є два важливі моменти: по-перше, рядок `let x = five();` показує, що ми використовуємо значення, яке повернула функція, для ініціалізації змінної. Оскільки функція `five` повертає `5`, цей рядок робить те саме, що й такий:

```rust
let x = 5;
```

По-друге, функція `п'ять` не має параметрів і визначає тип значення, що повертає, але тіло функції -- самотнє `5` без крапки з комою -- це вираз, значення якого ми хочемо повернути.

Подивімося інший приклад:

<span class="filename">Файл: src/main.rs</span>

```rust
{{#rustdoc_include ../listings/ch03-common-programming-concepts/no-listing-22-function-parameter-and-return/src/main.rs}}
```

Якщо виконати цей код, він виведе `The value of x is: 6`. Але якщо ми поставимо крапку з комою в кінець рядка `x + 1`, щоб він став не виразом, а інструкцією, ми дістанемо помилку:

<span class="filename">Файл: src/main.rs</span>

```rust,ignore,does_not_compile
{{#rustdoc_include ../listings/ch03-common-programming-concepts/no-listing-23-statements-dont-return-values/src/main.rs}}
```

Компіляція цього коду призводить до такої помилки:

```console
{{#include ../listings/ch03-common-programming-concepts/no-listing-23-statements-dont-return-values/output.txt}}
```

The main error message, `mismatched types`, reveals the core issue with this code. The definition of the function `plus_one` says that it will return an `i32`, but statements don’t evaluate to a value, which is expressed by `()`, the unit type. Therefore, nothing is returned, which contradicts the function definition and results in an error. In this output, Rust provides a message to possibly help rectify this issue: it suggests removing the semicolon, which would fix the error.
