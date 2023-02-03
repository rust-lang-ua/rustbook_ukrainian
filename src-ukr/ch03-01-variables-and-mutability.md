## Змінні і мутабельність

Як уже згадувалося у підрозділі [“Зберігання значень у змінних”][storing-values-with-variables]<!-- ignore --> , за замовчанням змінні є *немутабельними*. Це - один з численних штурханців, якими Rust заохочує вас писати код, що користується перевагами у безпеці та простоті написання конкретного коду, які надає Rust. З усім тим, ви все ж маєте можливість зробити змінні мутабельними. Дослідимо, як і чому Rust заохочує вас надавати перевагу немутабельності, та чому ви можете захотіти відмовитися від цього.

Якщо змінна є немутабельною, це означає, що відколи значення стає прив'язаним до імені, ви не можете змінити це значення. Щоб проілюструвати це, згенеруємо новий проєкт з назвою*variables* у вашій теці *projects* за допомогою `cargo new variables`.

Потім, у новоствореній теці *variables*, відкрийте *src/main.rs* і замініть його код цим, який поки що не компілюється:

<span class="filename">Файл: src/main.rs</span>

```rust,ignore,does_not_compile
{{#rustdoc_include ../listings/ch03-common-programming-concepts/no-listing-01-variables-are-immutable/src/main.rs}}
```

Збережіть і запустіть програму за допомогою `cargo run`. Ви маєте отримати повідомлення про помилку щодо помилки немутабельності, як показано на цьому виведенні:

```console
{{#include ../listings/ch03-common-programming-concepts/no-listing-01-variables-are-immutable/output.txt}}
```

Цей приклад показує, як компілятор допомагає вам знаходити помилки у програмах. Хоча повідомлення компілятора про помилки й можуть засмучувати, та вони лише означають, що ваша програма ще не робить те, що ви хотіли, у безпечний спосіб; вони *не* означають, що ви поганий програміст! Досвідчені растацеанці також отримують повідомлення про помилки від компілятора.

You received the error message `` cannot assign twice to immutable variable `x` `` because you tried to assign a second value to the immutable `x` variable.

Важливо, що ми отримали помилку часу компіляції, коли намагалися змінити значення, яке раніше визначили як немутабельне, тому що ця ситуація може призвести до вад у програмі. Якщо одна частина нашого коду працює з припущенням, що значення не буде змінене, а інша частина нашого коду змінює це значення, можливо, що перша частина коду буде робити не те, для чого вона була розроблена. Цю причину вад важко відслідкувати після виявлення, особливо коли другий фрагмент коду змінює значення лише *час від часу*. The Rust compiler guarantees that when you state that a value won’t change, it really won’t change, so you don’t have to keep track of it yourself. Ваш код стає легше зрозуміти.

Але мутабельність може бути дуже корисною і може бути зручнішим писати код з мутабельністю. Although variables are immutable by default, you can make them mutable by adding `mut` in front of the variable name as you did in [Chapter 2][storing-values-with-variables]<!-- ignore -->. Adding `mut` also conveys intent to future readers of the code by indicating that other parts of the code will be changing this variable’s value.

Наприклад, змінімо *src/main.rs* на такий код:

<span class="filename">Файл: src/main.rs</span>

```rust
{{#rustdoc_include ../listings/ch03-common-programming-concepts/no-listing-02-adding-mut/src/main.rs}}
```

Запустивши програму ми отримаємо:

```console
{{#include ../listings/ch03-common-programming-concepts/no-listing-02-adding-mut/output.txt}}
```

We’re allowed to change the value bound to `x` from `5` to `6` when `mut` is used. Остаточне рішення, використовувати мутабельність чи ні, належить вам і залежить від того, що ви вважаєте найочевиднішим у конкретній ситуації.

### Константи

Подібно до немутабельних змінних, *константи* так само є значенням, прив'язаним до імені, які не можна змінювати, але є кілька відмінностей між константами і змінними.

По-перше, не можна використовувати `mut` з константами. Константи не просто немутабельні за замовчанням, вони завжди немутабельні. Константи проголошуються ключовим словом `const` замість `let`, і тип значення *має* явно позначатися. We’ll cover types and type annotations in the next section, [“Data Types,”][data-types]<!-- ignore -->, so don’t worry about the details right now. Просто пам'ятайте, що тип констант треба зазначати завжди.

Constants can be declared in any scope, including the global scope, which makes them useful for values that many parts of code need to know about.

The last difference is that constants may be set only to a constant expression, not the result of a value that could only be computed at runtime.

Ось приклад проголошення константи:

```rust
const THREE_HOURS_IN_SECONDS: u32 = 60 * 60 * 3;
```

Константа зветься `THREE_HOURS_IN_SECONDS` і її значення встановлене в результат множення 60 (числа секунд у хвилині) на 60 (числа хвилин у годині) на 3 (числа годин, що ми хочемо порахувати у цій програмі). Угода про назви констант в Rust вимагає використання верхнього регістру із підкресленнями між словами. Компілятор здатний обчислити невеликий набір операцій під час компіляції, що дозволяє нам виписати це значення так, щоб його було легше зрозуміти та перевірити, замість того щоб встановлювати константі значення 10800. Зверніться до [Підрозділу Довідника Rust про обчислення констант][const-eval] за додатковою інформацією про те, які операції можна використовувати при проголошенні констант.

Constants are valid for the entire time a program runs, within the scope in which they were declared. This property makes constants useful for values in your application domain that multiple parts of the program might need to know about, such as the maximum number of points any player of a game is allowed to earn, or the speed of light.

Корисно давати назви жорстко заданим значенням, що використовуються у вашій програмі, позначаючи їх константами, щоб передати сенс цього значення тим, хто супроводжуватиме код. Це також корисно тим, що в коді буде тільки одне місце, яке буде необхідно змінити у разі потреби оновити жорстко задане значення.

### Затінення

Як ви бачили під час програмування гри - відгадайки у [Розділі 2]()<!-- ignore -->, можна проголошувати нову змінну із таким самим іменем, як і в раніше проголошеної змінної. Растацеанці кажуть, що перша змінна *затінена* другою, що означає, що при використанні змінної компілятор бачить лише другу змінну. По суті, друга змінна перекриває першу, перехоплюючи будь-яку згадку імені змінної на себе до тих пір, поки вона сама не буде затінена або область видимості не закінчиться. Ми можемо затінити змінну за допомогою ключового слова `let` та імені цієї змінної, ось так:

<span class="filename">Файл: src/main.rs</span>

```rust
{{#rustdoc_include ../listings/ch03-common-programming-concepts/no-listing-03-shadowing/src/main.rs}}
```

Ця програма спершу прив'язує `x` до значення `5`. Потім створює нову змінну `x`, повторюючи `let x =` і початкове значення та додає до нього `1`, так що значення `x` тепер `6`. Потім, у внутрішній області видимості, створеній фігурними дужками, третя інструкція `let` знову затінює `x` і створює нову змінну, домножуючи попереднє значення на `2`, щоб надати `x` значення `12`. Коли область видимості завершується, внутрішнє затінення теж завершується і `x` повертається до значення `6`. Якщо ми запустимо цю програму, вона виведе:

```console
{{#include ../listings/ch03-common-programming-concepts/no-listing-03-shadowing/output.txt}}
```

Shadowing is different from marking a variable as `mut` because we’ll get a compile-time error if we accidentally try to reassign to this variable without using the `let` keyword. Використовуючи `let`, ми можемо виконати кілька перетворень значення, але лишити змінну немутабельною виконання цих перетворень.

Інша різниця між `mut` та затіненням полягає в тому, що, оскільки коли ми пишемо знову ключове слово `let`, насправді ми створюємо нову змінну, тож можемо змінити тип значення, але залишити ім'я. Наприклад, хай наша програма просить користувача вказати, скільки пробілів має бути всередині якогось тексту, ввівши символи пробілу, але насправді ми хочемо зберігати це значення як число:

```rust
{{#rustdoc_include ../listings/ch03-common-programming-concepts/no-listing-04-shadowing-can-change-types/src/main.rs:here}}
```

Перша змінна `spaces` має стрічковий тип, а друга змінна `spaces` має числовий тип. Затінення, таким чином, позбавляє нас необхідності придумувати різні імена, на кшталт `spaces_str` та `spaces_num`; натомість, ми можемо заново використати простіше ім'я `spaces`. Але якщо ми спробуємо для цього скористатися `mut`, як показано далі, то дістанемо помилку часу компіляції:

```rust,ignore,does_not_compile
{{#rustdoc_include ../listings/ch03-common-programming-concepts/no-listing-05-mut-cant-change-types/src/main.rs:here}}
```

Помилка каже, що не можна змінювати тип змінної:

```console
{{#include ../listings/ch03-common-programming-concepts/no-listing-05-mut-cant-change-types/output.txt}}
```

Now that we’ve explored how variables work, let’s look at more data types they can have. ch02-00-guessing-game-tutorial.html#comparing-the-guess-to-the-secret-number
ch02-00-guessing-game-tutorial.html#comparing-the-guess-to-the-secret-number

[data-types]: ch03-02-data-types.html#data-types
[storing-values-with-variables]: ch02-00-guessing-game-tutorial.html#storing-values-with-variables
[storing-values-with-variables]: ch02-00-guessing-game-tutorial.html#storing-values-with-variables
[const-eval]: ../reference/const_eval.html
