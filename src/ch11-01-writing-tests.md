## Як писати тести

Тести - це Rust функції, які перевіряють, чи тестований код працює
очікуваним способом. Тіла тестових функцій зазвичай виконують
наступні три дії:

1. Встановити будь-яке потрібне значення або стан.
2. Запустити на виконання код, який ви хочете протестувати.
3. Переконатися, що отримані результати відповідають вашим очікуванням.

Давайте розглянемо функції, які надає Rust спеціально для написання тестів та
виконання вище зазначених дій, що включають  `test` атрибут, декілька макросів, а також атрибут
 `should_panic`.

### Структура тестуючої функції

У найпростішому випадку, тест у Rust - це функція що анотована за допомогою атрибута 
`test`. Атрибути - це метадані фрагментів Rust-коду; наприклад 
 атрибут `derive`, який ми використовували зі структурами у розділі 5. Для перетворення звичайної функції
на функцію-тест, додайте `#[test]` на рядку перед `fn`. Коли ви запускаєте ваші 
тести за допомогою команди `cargo test`, Rust будує бінарний файл, який запускає 
анотовані функції та звітує
чи виконалися тестові функції успішно або трапився збій.

Кожного разу, коли ми створюємо новий проєкт за допомогою Cargo, він автоматично генерує для нас тестовий модуль 
з тестовими функціями. Цей модуль надає вам шаблон
для написання тестів, а отже вам непотрібно кожного разу при створенні нового проекту
уточнювати їх структуру та синтаксис. Ви можете додати стільки тестових модулів
та тестових функцій, скільки забажаєте!

Перш ніж фактично протестувати будь-який код,
ми розглянемо деякі аспекти роботи тестів,
 експериментуючи з шаблоном тесту. Потім ми напишемо декілька реальних тестів,
 що запускають наш код та підтверджують, що його поведінка є правильною.

Давайте створимо новий проєкт бібліотеки під назвою `adder`, в якому додаються два числа:

```console
$ cargo new adder --lib
     Created library `adder` project
$ cd adder
```

Вміст файлу  *src/lib.rs* у вашій бібліотеці ` adder` має виглядати так
Лістінг 11-1.

<span class="filename">Filename: src/lib.rs</span>

<!-- manual-regeneration
cd listings/ch11-writing-automated-tests
rm -rf listing-11-01
cargo new --lib listing-11-01 --name adder
cd listing-11-01
cargo test
git co output.txt
cd ../../..
-->

```rust,noplayground
{{#rustdoc_include ../listings/ch11-writing-automated-tests/listing-11-01/src/lib.rs}}
```

<span class="caption">Listing 11-1: The test module and function generated
automatically by `cargo new`</span>

Наразі давайте проігноруємо два верхні рядки та зосередимося на функції. Зверніть увагу на
анотацію `#[test]`: Цей атрибут вказує на те, що функція є тестуючою,
та функціонал для запуску тестів ставитиметься до неї, як до теста. Ми також можемо мати нетестуючі функції
в модулі `tests`  щоб допомогти налаштувати типові сценарії або виконати загальні операції, 
 тому ми завжди повинні помічати за допомогою анотації які саме функції є тестуючими..

Тіло функції зі зразка використовує макрос `assert_eq!`  для ствердження того, що `result`,
який містить результат операції  додавання 2 та 2, дорівнює 4. Це ствердження служить 
типовим зразком формату для тесту. Давайте запустимо його 
та впевнимось, що тест проходить коректно.

Команда  `cargo test` запускає усі тести з нашого  проєкту, як показано у лістінгу
11-2.

```console
{{#include ../listings/ch11-writing-automated-tests/listing-11-01/output.txt}}
```

<span class="caption">Listing 11-2: The output from running the automatically
generated test</span>

Cargo скомпілював та запустив тест. Ми бачимо рядок `running 1 test`. наступний рядок 
показує ім'я згенерованої тестуючої функції `it_works`, а також що 
результат запуску тесту є `ok`. Загальний результат `test result: ok.`
означає, що усі тести пройшли успішно, а частина `1 passed; 0
failed` показує загальну кількість тестів що були пройдені успішно або завершилися зі збоєм.

Можна помітити деякі тести як ігноровані, тоді вони не будуть запускатися; ми розглянемо це далі у цьому розділі у [“Ignoring Some Tests Unless Specifically
Requested”][ignoring]<!-- ignore --> . Оскільки ми
не помічали жодного тесту для ігнорування, ми отрримуємо на виході `0 ignored`. Ми також можемо
додати до команди `cargo test` аргумент, щоб запускалися лише ті тести, що відповідають певному рядку; Це нназивається  *фільтрацією* та  ми поговоримо про це у  [“Running a
Subset of Tests by Name”][subset]<!-- ignore -->. Ми також не маємо
відфільтрованих тестів, тому вивід показує `0 filtered out`.

`0 measured` показує статистику тестів швидкодії.
Тести швидкодії на момент написання цієї статті доступні лише у нічних збірках Rust. Дивись 
[the documentation about benchmark tests][bench] для більш детальної інформації.

Наступна частина виводу  `Doc-tests adder` призначена для 
 відображення тестів документації. У нас поки що немає тестів документації,
але Rust може скомпілювати будь-які приклади коду з документації по нашому api.
Ця функція допомагає синхронізувати вашу документацію та код! Ми розглянемо 
як писати тести документації в  [“Documentation Comments as
Tests”][doc-comments]<!-- ignore --> у розділі 14. Зараз ми проігноруємо частину виводу, присвячену  
`Doc-tests`.

Давайте налаштуємо тест для відповідності нашим потребам. Спочатку змінимо ім'я тестової функції`it_works`  на інше, наприклад таке як  `exploration`:

<span class="filename">Filename: src/lib.rs</span>

```rust,noplayground
{{#rustdoc_include ../listings/ch11-writing-automated-tests/no-listing-01-changing-test-name/src/lib.rs}}
```

Далі знову запустимо  `cargo test`. Вивід тепер покаже  `exploration` замість `it_works`:

```console
{{#include ../listings/ch11-writing-automated-tests/no-listing-01-changing-test-name/output.txt}}
```

Тепер ми додамо інший тест, але цього разу він завершиться зі збоєм! Тести
завершуються зі збоєм коли щось у тестуючій функції викликає паніку. Кожний тест запускається в окремому
потоці, та коли головний потік бачить, що тестовий потік упав, то тест помічається як такий, що завершився аварійно. У розділі 9 ми розглядали найпростіший спосіб викликати паніку
за допомогою виклику макросу `panic!`. Створіть новий тест та назвіть тестуючу функцію
`another`, ваш файл  *src/lib.rs* буде виглядати як у лістінгу 11-3.

<span class="filename">Filename: src/lib.rs</span>

```rust,panics,noplayground
{{#rustdoc_include ../listings/ch11-writing-automated-tests/listing-11-03/src/lib.rs:here}}
```

<span class="caption">Listing 11-3: Adding a second test that will fail because
we call the `panic!` macro</span>

Запустіть тест знову, використовуючи `cargo test`. Вивід виглядатиме схоже на лістінг
11-4, який показує, що тест  `exploration` пройшов успішно, а `another` завершився зі збоєм.

```console
{{#include ../listings/ch11-writing-automated-tests/listing-11-03/output.txt}}
```

<span class="caption">Listing 11-4: Test results when one test passes and one
test fails</span>

Замість `ok`, рядок `test tests::another` відображає `FAILED`. Дві нові
секції з'явилися між результатами окремих тестів та загальними результатами: перша 
показує детальну причину того, що тест завершився аварійно. У цьому випадку ми отрирмали
те, що тест  `another` завершився зі збоєм тому, що  `panicked at 'Make this test fail'` у рядку  10 у  файлі *src/lib.rs*.
У наступній секції наведені назви тестів, що не пройшли, і це зручно коли у нас багато таких тестів та багато деталей про аварійне завершення.
 Ми можемо використати ім'я тесту для його подальшого відлагодження; ми поговоримо більше про запуск тестів у розділі
 [“Controlling How Tests Are Run”][controlling-how-tests-are-run]<!-- ignore--> .

У кінці відображається ітоговий результат тестування: в цілому результат нашого тесту `FAILED`. У нас
один тест пройшов, та один завершився зі збоєм.

Тепер, коли ви побачили, як виглядають результати тесту в різних сценаріях,
давайте розглянемо деякі макроси, крім `panic!`, які корисні в тестах.

### Перевірка результатів за допомогою макроса `assert!`

Макрос `assert!` , що надається стандартною бібліотекою, широко використовується 
для того, щоб впевнитися, що деяка умова  у тесті приймає значення `true`. Ми даємо 
макросу `assert!` аргумент, який обчислюється як вираз логічного типу. Якщо значення обчислюється як
`true`, нічого поганого не трапляється та тест вважається успішно пройденим. Якщо ж значення буде 'false'
макрос `assert!` викликає паніку `panic!`, що спричиняє аварійне завершення тесту. Використання макросу `assert!`
допомагає нам перевірити, чи працює наш код очікуваним способом.

У розділі 5, Лістінг 5-15, ми використовували структуру a `Rectangle`  та метод `can_hold`,
які повторюються у  лістінгу 11-5. Давайте розмістимо цей код у
файлі *src/lib.rs*, а далі напишемо декілька тестів, використовуючи  макрос  `assert!`.

<span class="filename">Filename: src/lib.rs</span>

```rust,noplayground
{{#rustdoc_include ../listings/ch11-writing-automated-tests/listing-11-05/src/lib.rs:here}}
```

<span class="caption">Listing 11-5: Using the `Rectangle` struct and its
`can_hold` method from Chapter 5</span>

 Метод `can_hold` повертає логічне значення, що означає, що це ідеальний варіант використання
для  макросу `assert!`. В лістінгу 11-6, ми пишемо тест, який перевіряє
`can_hold` method by creating a `Rectangle` instance that has a width of 8 and
a height of 7 and asserting that it can hold another `Rectangle` instance that
has a width of 5 and a height of 1.

<span class="filename">Filename: src/lib.rs</span>

```rust,noplayground
{{#rustdoc_include ../listings/ch11-writing-automated-tests/listing-11-06/src/lib.rs:here}}
```

<span class="caption">Listing 11-6: A test for `can_hold` that checks whether a
larger rectangle can indeed hold a smaller rectangle</span>

Note that we’ve added a new line inside the `tests` module: `use super::*;`.
The `tests` module is a regular module that follows the usual visibility rules
we covered in Chapter 7 in the [“Paths for Referring to an Item in the Module
Tree”][paths-for-referring-to-an-item-in-the-module-tree]<!-- ignore -->
section. Because the `tests` module is an inner module, we need to bring the
code under test in the outer module into the scope of the inner module. We use
a glob here so anything we define in the outer module is available to this
`tests` module.

We’ve named our test `larger_can_hold_smaller`, and we’ve created the two
`Rectangle` instances that we need. Then we called the `assert!` macro and
passed it the result of calling `larger.can_hold(&smaller)`. This expression is
supposed to return `true`, so our test should pass. Let’s find out!

```console
{{#include ../listings/ch11-writing-automated-tests/listing-11-06/output.txt}}
```

It does pass! Let’s add another test, this time asserting that a smaller
rectangle cannot hold a larger rectangle:

<span class="filename">Filename: src/lib.rs</span>

```rust,noplayground
{{#rustdoc_include ../listings/ch11-writing-automated-tests/no-listing-02-adding-another-rectangle-test/src/lib.rs:here}}
```

Because the correct result of the `can_hold` function in this case is `false`,
we need to negate that result before we pass it to the `assert!` macro. As a
result, our test will pass if `can_hold` returns `false`:

```console
{{#include ../listings/ch11-writing-automated-tests/no-listing-02-adding-another-rectangle-test/output.txt}}
```

Two tests that pass! Now let’s see what happens to our test results when we
introduce a bug in our code. We’ll change the implementation of the `can_hold`
method by replacing the greater-than sign with a less-than sign when it
compares the widths:

```rust,not_desired_behavior,noplayground
{{#rustdoc_include ../listings/ch11-writing-automated-tests/no-listing-03-introducing-a-bug/src/lib.rs:here}}
```

Running the tests now produces the following:

```console
{{#include ../listings/ch11-writing-automated-tests/no-listing-03-introducing-a-bug/output.txt}}
```

Our tests caught the bug! Because `larger.width` is 8 and `smaller.width` is
5, the comparison of the widths in `can_hold` now returns `false`: 8 is not
less than 5.

### Testing Equality with the `assert_eq!` and `assert_ne!` Macros

A common way to verify functionality is to test for equality between the result
of the code under test and the value you expect the code to return. You could
do this using the `assert!` macro and passing it an expression using the `==`
operator. However, this is such a common test that the standard library
provides a pair of macros—`assert_eq!` and `assert_ne!`—to perform this test
more conveniently. These macros compare two arguments for equality or
inequality, respectively. They’ll also print the two values if the assertion
fails, which makes it easier to see *why* the test failed; conversely, the
`assert!` macro only indicates that it got a `false` value for the `==`
expression, without printing the values that led to the `false` value.

In Listing 11-7, we write a function named `add_two` that adds `2` to its
parameter, then we test this function using the `assert_eq!` macro.

<span class="filename">Filename: src/lib.rs</span>

```rust,noplayground
{{#rustdoc_include ../listings/ch11-writing-automated-tests/listing-11-07/src/lib.rs}}
```

<span class="caption">Listing 11-7: Testing the function `add_two` using the
`assert_eq!` macro</span>

Let’s check that it passes!

```console
{{#include ../listings/ch11-writing-automated-tests/listing-11-07/output.txt}}
```

We pass `4` as the argument to `assert_eq!`, which is equal to the result of
calling `add_two(2)`. The line for this test is `test tests::it_adds_two ...
ok`, and the `ok` text indicates that our test passed!

Let’s introduce a bug into our code to see what `assert_eq!` looks like when it
fails. Change the implementation of the `add_two` function to instead add `3`:

```rust,not_desired_behavior,noplayground
{{#rustdoc_include ../listings/ch11-writing-automated-tests/no-listing-04-bug-in-add-two/src/lib.rs:here}}
```

Run the tests again:

```console
{{#include ../listings/ch11-writing-automated-tests/no-listing-04-bug-in-add-two/output.txt}}
```

Our test caught the bug! The `it_adds_two` test failed, and the message tells
us that the assertion that fails was `` assertion failed: `(left == right)` ``
and what the `left` and `right` values are. This message helps us start
debugging: the `left` argument was `4` but the `right` argument, where we had
`add_two(2)`, was `5`. You can imagine that this would be especially helpful
when we have a lot of tests going on.

Note that in some languages and test frameworks, the parameters to equality
assertion functions are called `expected` and `actual`, and the order in which
we specify the arguments matters. However, in Rust, they’re called `left` and
`right`, and the order in which we specify the value we expect and the value
the code produces doesn’t matter. We could write the assertion in this test as
`assert_eq!(add_two(2), 4)`, which would result in the same failure message
that displays `` assertion failed: `(left == right)` ``.

The `assert_ne!` macro will pass if the two values we give it are not equal and
fail if they’re equal. This macro is most useful for cases when we’re not sure
what a value *will* be, but we know what the value definitely *shouldn’t* be.
For example, if we’re testing a function that is guaranteed to change its input
in some way, but the way in which the input is changed depends on the day of
the week that we run our tests, the best thing to assert might be that the
output of the function is not equal to the input.

Under the surface, the `assert_eq!` and `assert_ne!` macros use the operators
`==` and `!=`, respectively. When the assertions fail, these macros print their
arguments using debug formatting, which means the values being compared must
implement the `PartialEq` and `Debug` traits. All primitive types and most of
the standard library types implement these traits. For structs and enums that
you define yourself, you’ll need to implement `PartialEq` to assert equality of
those types. You’ll also need to implement `Debug` to print the values when the
assertion fails. Because both traits are derivable traits, as mentioned in
Listing 5-12 in Chapter 5, this is usually as straightforward as adding the
`#[derive(PartialEq, Debug)]` annotation to your struct or enum definition. See
Appendix C, [“Derivable Traits,”][derivable-traits]<!-- ignore --> for more
details about these and other derivable traits.

### Adding Custom Failure Messages

You can also add a custom message to be printed with the failure message as
optional arguments to the `assert!`, `assert_eq!`, and `assert_ne!` macros. Any
arguments specified after the required arguments are passed along to the
`format!` macro (discussed in Chapter 8 in the [“Concatenation with the `+`
Operator or the `format!`
Macro”][concatenation-with-the--operator-or-the-format-macro]<!-- ignore -->
section), so you can pass a format string that contains `{}` placeholders and
values to go in those placeholders. Custom messages are useful for documenting
what an assertion means; when a test fails, you’ll have a better idea of what
the problem is with the code.

For example, let’s say we have a function that greets people by name and we
want to test that the name we pass into the function appears in the output:

<span class="filename">Filename: src/lib.rs</span>

```rust,noplayground
{{#rustdoc_include ../listings/ch11-writing-automated-tests/no-listing-05-greeter/src/lib.rs}}
```

The requirements for this program haven’t been agreed upon yet, and we’re
pretty sure the `Hello` text at the beginning of the greeting will change. We
decided we don’t want to have to update the test when the requirements change,
so instead of checking for exact equality to the value returned from the
`greeting` function, we’ll just assert that the output contains the text of the
input parameter.

Now let’s introduce a bug into this code by changing `greeting` to exclude
`name` to see what the default test failure looks like:

```rust,not_desired_behavior,noplayground
{{#rustdoc_include ../listings/ch11-writing-automated-tests/no-listing-06-greeter-with-bug/src/lib.rs:here}}
```

Running this test produces the following:

```console
{{#include ../listings/ch11-writing-automated-tests/no-listing-06-greeter-with-bug/output.txt}}
```

This result just indicates that the assertion failed and which line the
assertion is on. A more useful failure message would print the value from the
`greeting` function. Let’s add a custom failure message composed of a format
string with a placeholder filled in with the actual value we got from the
`greeting` function:

```rust,ignore
{{#rustdoc_include ../listings/ch11-writing-automated-tests/no-listing-07-custom-failure-message/src/lib.rs:here}}
```

Now when we run the test, we’ll get a more informative error message:

```console
{{#include ../listings/ch11-writing-automated-tests/no-listing-07-custom-failure-message/output.txt}}
```

We can see the value we actually got in the test output, which would help us
debug what happened instead of what we were expecting to happen.

### Checking for Panics with `should_panic`

In addition to checking return values, it’s important to check that our code
handles error conditions as we expect. For example, consider the `Guess` type
that we created in Chapter 9, Listing 9-13. Other code that uses `Guess`
depends on the guarantee that `Guess` instances will contain only values
between 1 and 100. We can write a test that ensures that attempting to create a
`Guess` instance with a value outside that range panics.

We do this by adding the attribute `should_panic` to our test function. The
test passes if the code inside the function panics; the test fails if the code
inside the function doesn’t panic.

Listing 11-8 shows a test that checks that the error conditions of `Guess::new`
happen when we expect them to.

<span class="filename">Filename: src/lib.rs</span>

```rust,noplayground
{{#rustdoc_include ../listings/ch11-writing-automated-tests/listing-11-08/src/lib.rs}}
```

<span class="caption">Listing 11-8: Testing that a condition will cause a
`panic!`</span>

We place the `#[should_panic]` attribute after the `#[test]` attribute and
before the test function it applies to. Let’s look at the result when this test
passes:

```console
{{#include ../listings/ch11-writing-automated-tests/listing-11-08/output.txt}}
```

Looks good! Now let’s introduce a bug in our code by removing the condition
that the `new` function will panic if the value is greater than 100:

```rust,not_desired_behavior,noplayground
{{#rustdoc_include ../listings/ch11-writing-automated-tests/no-listing-08-guess-with-bug/src/lib.rs:here}}
```

When we run the test in Listing 11-8, it will fail:

```console
{{#include ../listings/ch11-writing-automated-tests/no-listing-08-guess-with-bug/output.txt}}
```

We don’t get a very helpful message in this case, but when we look at the test
function, we see that it’s annotated with `#[should_panic]`. The failure we got
means that the code in the test function did not cause a panic.

Tests that use `should_panic` can be imprecise. A `should_panic` test would
pass even if the test panics for a different reason from the one we were
expecting. To make `should_panic` tests more precise, we can add an optional
`expected` parameter to the `should_panic` attribute. The test harness will
make sure that the failure message contains the provided text. For example,
consider the modified code for `Guess` in Listing 11-9 where the `new` function
panics with different messages depending on whether the value is too small or
too large.

<span class="filename">Filename: src/lib.rs</span>

```rust,noplayground
{{#rustdoc_include ../listings/ch11-writing-automated-tests/listing-11-09/src/lib.rs:here}}
```

<span class="caption">Listing 11-9: Testing for a `panic!` with a panic message
containing a specified substring</span>

This test will pass because the value we put in the `should_panic` attribute’s
`expected` parameter is a substring of the message that the `Guess::new`
function panics with. We could have specified the entire panic message that we
expect, which in this case would be `Guess value must be less than or equal to
100, got 200.` What you choose to specify depends on how much of the panic
message is unique or dynamic and how precise you want your test to be. In this
case, a substring of the panic message is enough to ensure that the code in the
test function executes the `else if value > 100` case.

To see what happens when a `should_panic` test with an `expected` message
fails, let’s again introduce a bug into our code by swapping the bodies of the
`if value < 1` and the `else if value > 100` blocks:

```rust,ignore,not_desired_behavior
{{#rustdoc_include ../listings/ch11-writing-automated-tests/no-listing-09-guess-with-panic-msg-bug/src/lib.rs:here}}
```

This time when we run the `should_panic` test, it will fail:

```console
{{#include ../listings/ch11-writing-automated-tests/no-listing-09-guess-with-panic-msg-bug/output.txt}}
```

The failure message indicates that this test did indeed panic as we expected,
but the panic message did not include the expected string `'Guess value must be
less than or equal to 100'`. The panic message that we did get in this case was
`Guess value must be greater than or equal to 1, got 200.` Now we can start
figuring out where our bug is!

### Using `Result<T, E>` in Tests

Our tests so far all panic when they fail. We can also write tests that use
`Result<T, E>`! Here’s the test from Listing 11-1, rewritten to use `Result<T,
E>` and return an `Err` instead of panicking:

```rust,noplayground
{{#rustdoc_include ../listings/ch11-writing-automated-tests/no-listing-10-result-in-tests/src/lib.rs}}
```

The `it_works` function now has the `Result<(), String>` return type. In the
body of the function, rather than calling the `assert_eq!` macro, we return
`Ok(())` when the test passes and an `Err` with a `String` inside when the test
fails.

Writing tests so they return a `Result<T, E>` enables you to use the question
mark operator in the body of tests, which can be a convenient way to write
tests that should fail if any operation within them returns an `Err` variant.

You can’t use the `#[should_panic]` annotation on tests that use `Result<T,
E>`. To assert that an operation returns an `Err` variant, *don’t* use the
question mark operator on the `Result<T, E>` value. Instead, use
`assert!(value.is_err())`.

Now that you know several ways to write tests, let’s look at what is happening
when we run our tests and explore the different options we can use with `cargo
test`.

[concatenation-with-the--operator-or-the-format-macro]:
ch08-02-strings.html#concatenation-with-the--operator-or-the-format-macro
[bench]: ../unstable-book/library-features/test.html
[ignoring]: ch11-02-running-tests.html#ignoring-some-tests-unless-specifically-requested
[subset]: ch11-02-running-tests.html#running-a-subset-of-tests-by-name
[controlling-how-tests-are-run]:
ch11-02-running-tests.html#controlling-how-tests-are-run
[derivable-traits]: appendix-03-derivable-traits.html
[doc-comments]: ch14-02-publishing-to-crates-io.html#documentation-comments-as-tests
[paths-for-referring-to-an-item-in-the-module-tree]: ch07-03-paths-for-referring-to-an-item-in-the-module-tree.html
