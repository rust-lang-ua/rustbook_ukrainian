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

Усе це коректні значення `String`.

### Зміна стрічок

`String` може зростати і її вміст може змінюватися, як вміст `Vec<T>`, якщо ви додасте у неї ще дані. На додачу, ви можете для зручності використовувати оператор `+` чи макрос `format!` для конкатенації значень `String`.

#### Додавання до String методами `push_str` і `push`

Ми можемо збільшити `String` за допомогою методу `push_str`, додавши стрічковий слайс, як показано в Блоці коду 8-15.

```rust
{{#rustdoc_include ../listings/ch08-common-collections/listing-08-15/src/main.rs:here}}
```


<span class="caption">Блок коду 8-15: додавання стрічкового слайсу до `String` за допомогою методу `push_str`</span>

Після цих двох рядків `s` міститиме `foobar`. Метод `push_str` приймає стрічковий слайс, бо ми не обов'язково хочемо приймати володіння параметром. Наприклад, у коді з Блоку коду 8-16, ми хочемо мати змогу використовувати `s2` після додавання його вмісту до `s1`.

```rust
{{#rustdoc_include ../listings/ch08-common-collections/listing-08-16/src/main.rs:here}}
```


<span class="caption">Блок коду 8-16: використання стрічкового слайсу після додавання його вмісту до `String`</span>

Якби метод `push_str` взяв володіння `s2`, ми б не змогли вивести його значення в останньому рядку. Але цей код працює, як ми очікуємо!

Метод `push` приймає параметром один символ і додає його до `String`. Блок коду 8-17 додає букву "l" до `String` за допомогою методу `push`.

```rust
{{#rustdoc_include ../listings/ch08-common-collections/listing-08-17/src/main.rs:here}}
```


<span class="caption">Блок коду 8-17: Додавання одного символу до значення `String` за допомогою `push`</span>

У результаті, `s` міститиме `lol`.

#### Конкатенація за допомогою оператора `+` і макроса `format!`

Часто вам треба поєднати дві стрічки. Один зі способів зробити це - використати оператор `+`, як показано в Блоці коду 8-18.

```rust
{{#rustdoc_include ../listings/ch08-common-collections/listing-08-18/src/main.rs:here}}
```


<span class="caption">Блок коду 8-18: використання оператора `+` для об'єднання двох значень `String` у нову `String`</span>

Стрічка `s3` міститиме `Hello, world!`. Причина, чому `s1` більше не дійсна після додавання, і причина, чому ми використовували посилання на `s2`, стосується сигнатури методу, викликаного, коли ми скористалися оператором `+`. Оператор `+` використовує метод `add`, сигнатура якого виглядає приблизно так:

```rust,ignore
fn add(self, s: &str) -> String {
```

У стандартній бібліотеці ви побачите `add`, визначений за допомогою узагальнених і асоційованих типів. Тут ми підставили конкретні типи, що й стається, коли ми викликаємо цей метод зі значеннями `String`. Узагальнені типи ми обговоримо у Розділі 10. Ця сигнатура дає нам підказки, потрібні для розуміння тонких місць оператора `+`.

По-перше, `s2` має `&`, що означає, що ми додаємо *посилання* на другу стрічку до першої стрічки. Так зроблено через параметр `s` у функції `add`: ми можемо лише додати `&str` до `String`; ми не можемо скласти разом два значення `String`. Але чекайте — типом `&s2` є `&String`, а не `&str`, як зазначено в другому параметрі `add`. То чому ж Блок коду 8-18 компілюється?

Причина, з якої ми можемо використовувати `&s2` у виклику `add` полягає в тому, що компілятор може *привести* аргумент `&String` до `&str`. Коли ми викликаємо метод `add`, Rust використовує *приведення розіменування*, що тут перетворює `&s2` у `&s2[..]`. Ми обговоримо приведення розіменування глибше в Розділі 15. Оскільки `add` не перебирає володіння параметром `s`, `s2` все ще буде коректною `String` після цієї операції.

По-друге, як ми бачимо в сигнатурі, `add` бере володіння над `self`, бо `self` *не* має `&`. Це означає, що `s1` у Блоці коду 8-18 буде перенесено у виклику `add` і після цього більше не буде дійсним. Таким чином, хоч `let s3 = s1 + &s2;` виглядає, ніби він копіює обидві стрічки і створює нову, ця інструкція насправді бере володіння `s1`, додає копію вмісту `s2`, а потім повертає володіння результатом. Іншими словами, він виглядає, ніби створює багато копій, але насправді ні; реалізація ефективніша за копіювання.

Якщо нам потрібно об'єднати декілька стрічок, поведінка оператора `+` стає громіздкою:

```rust
{{#rustdoc_include ../listings/ch08-common-collections/no-listing-01-concat-multiple-strings/src/main.rs:here}}
```

У цьому місці `s` буде `tic-tac-toe`. За усіма цими символами `+` і `"` стає важко побачити, що відбувається. Для складнішого комбінування стрічок ми можемо замість цього скористатися макросом `format!`:

```rust
{{#rustdoc_include ../listings/ch08-common-collections/no-listing-02-format/src/main.rs:here}}
```

Цей код також надає `s` значення `tic-tac-toe`. Макрос `format!` працює подібно до `println!`, але замість виведення на екран, повертає `String` з відповідним вмістом. Версію коду, що використовує `format!`, значно простіше читати, і код, що згенеровано макросом `format!`, використовує посилання, тому цей виклик не перебирає володіння жодним зі своїх параметрів.

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
