## Зберігання тексту у кодуванні UTF-8 в стрічках

We talked about strings in Chapter 4, but we’ll look at them in more depth now. New Rustaceans commonly get stuck on strings for a combination of three reasons: Rust’s propensity for exposing possible errors, strings being a more complicated data structure than many programmers give them credit for, and UTF-8. These factors combine in a way that can seem difficult when you’re coming from other programming languages.

We discuss strings in the context of collections because strings are implemented as a collection of bytes, plus some methods to provide useful functionality when those bytes are interpreted as text. In this section, we’ll talk about the operations on `String` that every collection type has, such as creating, updating, and reading. We’ll also discuss the ways in which `String` is different from the other collections, namely how indexing into a `String` is complicated by the differences between how people and computers interpret `String` data.

### What Is a String?

We’ll first define what we mean by the term *string*. Rust has only one string type in the core language, which is the string slice `str` that is usually seen in its borrowed form `&str`. In Chapter 4, we talked about *string slices*, which are references to some UTF-8 encoded string data stored elsewhere. String literals, for example, are stored in the program’s binary and are therefore string slices.

The `String` type, which is provided by Rust’s standard library rather than coded into the core language, is a growable, mutable, owned, UTF-8 encoded string type. When Rustaceans refer to “strings” in Rust, they might be referring to either the `String` or the string slice `&str` types, not just one of those types. Although this section is largely about `String`, both types are used heavily in Rust’s standard library, and both `String` and string slices are UTF-8 encoded.

### Creating a New String

Many of the same operations available with `Vec<T>` are available with `String` as well, because `String` is actually implemented as a wrapper around a vector of bytes with some extra guarantees, restrictions, and capabilities. An example of a function that works the same way with `Vec<T>` and `String` is the `new` function to create an instance, shown in Listing 8-11.

```rust
{{#rustdoc_include ../listings/ch08-common-collections/listing-08-11/src/main.rs:here}}
```

<span class="caption">Listing 8-11: Creating a new, empty `String`</span>

This line creates a new empty string called `s`, which we can then load data into. Often, we’ll have some initial data that we want to start the string with. For that, we use the `to_string` method, which is available on any type that implements the `Display` trait, as string literals do. Listing 8-12 shows two examples.

```rust
{{#rustdoc_include ../listings/ch08-common-collections/listing-08-12/src/main.rs:here}}
```


<span class="caption">Listing 8-12: Using the `to_string` method to create a `String` from a string literal</span>

This code creates a string containing `initial contents`.

We can also use the function `String::from` to create a `String` from a string literal. The code in Listing 8-13 is equivalent to the code from Listing 8-12 that uses `to_string`.

```rust
{{#rustdoc_include ../listings/ch08-common-collections/listing-08-13/src/main.rs:here}}
```


<span class="caption">Listing 8-13: Using the `String::from` function to create a `String` from a string literal</span>

Because strings are used for so many things, we can use many different generic APIs for strings, providing us with a lot of options. Some of them can seem redundant, but they all have their place! In this case, `String::from` and `to_string` do the same thing, so which you choose is a matter of style and readability.

Remember that strings are UTF-8 encoded, so we can include any properly encoded data in them, as shown in Listing 8-14.

```rust
{{#rustdoc_include ../listings/ch08-common-collections/listing-08-14/src/main.rs:here}}
```


<span class="caption">Listing 8-14: Storing greetings in different languages in strings</span>

All of these are valid `String` values.

### Updating a String

A `String` can grow in size and its contents can change, just like the contents of a `Vec<T>`, if you push more data into it. In addition, you can conveniently use the `+` operator or the `format!` macro to concatenate `String` values.

#### Appending to a String with `push_str` and `push`

We can grow a `String` by using the `push_str` method to append a string slice, as shown in Listing 8-15.

```rust
{{#rustdoc_include ../listings/ch08-common-collections/listing-08-15/src/main.rs:here}}
```


<span class="caption">Listing 8-15: Appending a string slice to a `String` using the `push_str` method</span>

After these two lines, `s` will contain `foobar`. The `push_str` method takes a string slice because we don’t necessarily want to take ownership of the parameter. For example, in the code in Listing 8-16, we want to be able to use `s2` after appending its contents to `s1`.

```rust
{{#rustdoc_include ../listings/ch08-common-collections/listing-08-16/src/main.rs:here}}
```


<span class="caption">Listing 8-16: Using a string slice after appending its contents to a `String`</span>

If the `push_str` method took ownership of `s2`, we wouldn’t be able to print its value on the last line. However, this code works as we’d expect!

The `push` method takes a single character as a parameter and adds it to the `String`. Listing 8-17 adds the letter “l” to a `String` using the `push` method.

```rust
{{#rustdoc_include ../listings/ch08-common-collections/listing-08-17/src/main.rs:here}}
```


<span class="caption">Listing 8-17: Adding one character to a `String` value using `push`</span>

As a result, `s` will contain `lol`.

#### Concatenation with the `+` Operator or the `format!` Macro

Often, you’ll want to combine two existing strings. One way to do so is to use the `+` operator, as shown in Listing 8-18.

```rust
{{#rustdoc_include ../listings/ch08-common-collections/listing-08-18/src/main.rs:here}}
```


<span class="caption">Listing 8-18: Using the `+` operator to combine two `String` values into a new `String` value</span>

The string `s3` will contain `Hello, world!`. The reason `s1` is no longer valid after the addition, and the reason we used a reference to `s2`, has to do with the signature of the method that’s called when we use the `+` operator. The `+` operator uses the `add` method, whose signature looks something like this:

```rust,ignore
fn add(self, s: &str) -> String {
```

In the standard library, you'll see `add` defined using generics and associated types. Here, we’ve substituted in concrete types, which is what happens when we call this method with `String` values. We’ll discuss generics in Chapter 10. This signature gives us the clues we need to understand the tricky bits of the `+` operator.

First, `s2` has an `&`, meaning that we’re adding a *reference* of the second string to the first string. This is because of the `s` parameter in the `add` function: we can only add a `&str` to a `String`; we can’t add two `String` values together. But wait—the type of `&s2` is `&String`, not `&str`, as specified in the second parameter to `add`. So why does Listing 8-18 compile?

The reason we’re able to use `&s2` in the call to `add` is that the compiler can *coerce* the `&String` argument into a `&str`. When we call the `add` method, Rust uses a *deref coercion*, which here turns `&s2` into `&s2[..]`. We’ll discuss deref coercion in more depth in Chapter 15. Because `add` does not take ownership of the `s` parameter, `s2` will still be a valid `String` after this operation.

Second, we can see in the signature that `add` takes ownership of `self`, because `self` does *not* have an `&`. This means `s1` in Listing 8-18 will be moved into the `add` call and will no longer be valid after that. So although `let s3 = s1 + &s2;` looks like it will copy both strings and create a new one, this statement actually takes ownership of `s1`, appends a copy of the contents of `s2`, and then returns ownership of the result. In other words, it looks like it’s making a lot of copies but isn’t; the implementation is more efficient than copying.

If we need to concatenate multiple strings, the behavior of the `+` operator gets unwieldy:

```rust
{{#rustdoc_include ../listings/ch08-common-collections/no-listing-01-concat-multiple-strings/src/main.rs:here}}
```

At this point, `s` will be `tic-tac-toe`. With all of the `+` and `"` characters, it’s difficult to see what’s going on. For more complicated string combining, we can instead use the `format!` macro:

```rust
{{#rustdoc_include ../listings/ch08-common-collections/no-listing-02-format/src/main.rs:here}}
```

This code also sets `s` to `tic-tac-toe`. The `format!` macro works like `println!`, but instead of printing the output to the screen, it returns a `String` with the contents. The version of the code using `format!` is much easier to read, and the code generated by the `format!` macro uses references so that this call doesn’t take ownership of any of its parameters.

### Індексація стрічок

У багатьох інших мовах програмування доступ до окремих символів у стрічці за допомогою індексу є припустимою і поширеною операцією. Однак, якщо ви спробуєте отримати шматки `String` за допомогою синтаксису індексів у Rust, то отримаєте помилку. Розглянемо неправильний код у Блоці коду 8-19.

```rust,ignore,does_not_compile
{{#rustdoc_include ../listings/ch08-common-collections/listing-08-19/src/main.rs:here}}
```


<span class="caption">Блок коду 8-19: спроба використати синтаксис індексів для String</span>

Цей код призведе до наступної помилки:

```console
{{#include ../listings/ch08-common-collections/listing-08-19/output.txt}}
```

Помилка і примітка кажуть самі за себе: стрічки у Rust не підтримують індексацію. Але чому ні? Щоб відповісти на це запитання, нам потрібно обговорити, як Rust зберігає стрічки в пам'яті.

#### Внутрішнє представлення

`String` є обгорткою `Vec<u8>`. Подивімося на деякі зразки правильно кодованих у UTF-8 стрічок з Блоку коду 8-14. Спершу, цей:

```rust
{{#rustdoc_include ../listings/ch08-common-collections/listing-08-14/src/main.rs:spanish}}
```

У цьому випадку `len` буде 4, це означає, що вектор, що зберігає стрічку "Hola", має довжину 4 байти. Кожна з цих літер займає 1 байт в кодуванні в UTF-8. Однак наступний рядок може вас здивувати.

```rust
{{#rustdoc_include ../listings/ch08-common-collections/listing-08-14/src/main.rs:ukrainian}}
```

Якщо вас спитати, якої довжини стрічка, ви можете сказати 6. Насправді Rust відповість, що 12 - це число байтів, потрібних, щоб закодувати "Привіт" у UTF-8, оскільки кожен скалярне значення Unicode у цій стрічці займає 2 байти пам'яті. Таким чином, індекс по байтах стрічки не завжди буде співвідноситися з припустимим скалярним значенням Unicode. Для демонстрації, розгляньмо цей некоректний код Rust:

```rust,ignore,does_not_compile
let hello = "Привіт";
let answer = &hello[0];
```

Ви вже знаєте, що `answer` не буде `П`, першою літерою. У кодуванні UTF-8 перший байт `П` буде `208`, а другий `159`, тож здається, що `answer` має бути `208`, але `208` сам по собі не є допустимим символом. Швидше за все, користувач, що запитує першу літеру у стрічці, не хоче отримати `208`; однак за індексом 0 Rust має лише ці дані. Користувачі, як правило, не хочуть отримувати значення байтів, навіть якщо стрічка містить лише латинські літери: якби `&"hello"[0]` було коректним кодом, що повертає значення байта, він усе одно поверне `104`, а не `h`.

Отже, відповідь полягає в тому, щоб уникнути повертання неочікуваного значення, що спричинить помилки, які можуть не бути виявлені негайно, і Rust узагалі не компілює цей код, запобігаючи непорозумінням на ранньому етапі процесу розробки.

#### Байти, скалярні значення і кластери графем! Божечки!

Ще одна особливість UTF-8 полягає у тому, що насправді існує три способи поглянути на стрічки з точки зору Rust: як на байти, скалярні значення та графеми кластери (найближче до того, що ми називаємо *літерами*).

Скажімо, слово мовою Гінді “नमस्ते”, записане письмом деванаґарі, зберігається як вектор значень `u8`, що виглядає ось так:

```text
[224, 164, 168, 224, 164, 174, 224, 164, 184, 224, 165, 141, 224, 164, 164,
224, 165, 135]
```

Це 18 байтів і так комп'ютери кінець-кінцем зберігають ці дані. Якщо ми подивимося на них як на скалярні значення Unicode, тобто те, чим є тип `char` у Rust, ці байти виглядають наступним чином:

```text
['न', 'म', 'स', '्', 'त', 'े']
```

Тут є шість значень `char`, але четвертий і шостий — не літери: це діакритичні знаки, які самостійно не мають жодного сенсу. Нарешті, якщо ми подивимось на них як на кластери графем, то отримаємо те, що людина назвала б чотирма літерами, які складають слово на Гінді:

```text
["न", "म", "स्", "ते"]
```

Rust надає різні способи інтерпретації необроблених даних стрічок, збережених комп'ютером, щоб кожна програма могла обрати потрібну їй інтерпретацію, неважливо якою людською мовою є ці дані.

Останньою причиною, чому Rust не дозволяє нам індексувати `String` для отримання символу, є те, що очікується, що операція індексації завжди займає постійний час (O(1)). Але неможливо гарантувати таку продуктивність для `String`, тому що Rust повинен проглянути вміст з початку до індексу, щоб визначити, скільки там було валідних символів.

### Слайси зі стрічок

Індексація стрічки часто є поганою ідеєю, бо не зрозуміло, яке значення має повертати операція індексування: байт, символ, кластер графем чи стріковий слайс. Тому, якщо вам дійсно треба використати індекси для створення стрічкових слайсів, Rust просить вас бути більш конкретним.

Замість індексування за допомогою `[]` з одним числом, ви можете використовувати `[]` з діапазоном, щоб створити стрічковий слайс, що містить певні байти:

```rust
let hello = "Привіт";

let s = &hello[0..4];
```

Тут `s` буде `&str`, що містить перші 4 байти стрічки. Раніше ми згадували, що кожен з цих символів має 2 байти, а це означає, що `s` буде `Пр`.

Якби ми спробували створити слайс з частини байтів символу чимось на кшталт `&hello[0..1]`, то Rust запанікував би під час виконання, так само, якби ми задали неправильний індекс для доступу до вектора:

```console
{{#include ../listings/ch08-common-collections/output-only-01-not-char-boundary/output.txt}}
```

Ви маєте використовувати діапазони для створення стрічкових слайсів з обережністю, тому що так може зламати програму.

### Методи для ітерації по стрічках

Найкращий спосіб оперувати фрагментами стрічкок - це чітко визначити, потрібні вам символи чи байти. Для роботи з окремими скалярними значеннями Unicode використовуйте метод `chars`. Виклик `chars` для “Пр" виділяє і повертає два значення типу `char`, і ви можете ітерувати результатом для доступу до кожного елемента:

```rust
for c in "Пр".chars() {
    println!("{}", c);
}
```

Цей код виводить на екран наступне:

```text
П
р
```

Альтернатива, метод `bytes` повертає кожен необроблений байт, що може бути більш придатним для вашої задачі:

```rust
for b in "Пр".bytes() {
    println!("{}", b);
}
```

Цей код виведе чотири байти, з яких складається ця стрічка:

```text
208
159
209
128
```

Але не забувайте, що припустимі скалярні значення Unicode можуть бути складені з більш ніж 1 байту.

Отримання кластерів графем зі стрічок, як у письмі деванагарі, є складним, тому ця функціональність не надається стандартною бібліотекою. На [crates.io](https://crates.io/) є відповідні крейти,<!-- ignore --> якщо ви потребуєте такого функціонала.

### Стрічки не такі прості

Підсумуємо: стрічки є складними. Різні мови програмування роблять різні вибори стосовно того, як представити цю складність програмісту. Rust обрав коректну обробку даних у `String` поведінкою за замовчуванням для всіх програм Rust, що означає, що програмісти повинні краще продумувати заздалегідь обробку даних UTF-8. Цей компроміс розкриває більшу частину складності стрічок, ніж зазвичай в інших мовах програмування, але це убезпечує вас від помилок із символами поза межами ASCII пізніше у життєвому циклі розробки.

Доброю новиною є те, що стандартна бібліотека пропонує велику кількість функціонала, побудованого на типах `String` і `&str`, щоб допомогти правильно впоратися з цими складними ситуаціями. Обов'язково перевірте документацію про корисні методи, такі як `contains` для пошуку в стрічці і `replace` на заміну частин стрічки на іншу стрічку.

Перейдемо на щось менш складне: хеш-відображення!
