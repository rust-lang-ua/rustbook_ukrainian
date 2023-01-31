## Типи даних

Кожне значення в Rust має певний *тип даних*, який каже Rust, якого виду це дані, щоб мова знала, як працювати з цими даними. Ми поглянемо на дві категорії типів даних: скалярні і складені.

Пам'ятайте, що Rust - *статично типізована* мова, тобто типи всіх змінних має бути відомим під час компіляції. Компілятор зазвичай може вивести, який тип ми хочемо використати, виходячи зі значення і того, як ми його використовуємо. У випадках, коли може підійти кілька типів, наприклад коли якщо ми перетворювали `String` на числовий тип за допомогою `parse` у підрозділі [“Порівняння здогадки з таємним числом”]()<!-- ignore --> Розділу 2, ми маємо додавати анотацію типу, ось так:

```rust
let guess: u32 = "42".parse().expect("Not a number!");
```

If we don’t add the `: u32` type annotation shown in the preceding code, Rust will display the following error, which means the compiler needs more information from us to know which type we want to use:

```console
{{#include ../listings/ch03-common-programming-concepts/output-only-01-no-type-annotations/output.txt}}
```

Ви побачите різні анотації типів для інших типів даних.

### Скалярні типи

*Скалярний* тип представляє єдине значення. У Rust є чотири первинні скалярні типи: цілі, числа з рухомою комою, булівські та символи. Ви можете впізнати їх з інших мов програмування. Гайда подивимося, як вони працюють у Rust.

#### Цілі типи

*Ціле* - це число без дробової частини. Ви використали один цілий тип у Розділі 2, а саме `u32`. This type declaration indicates that the value it’s associated with should be an unsigned integer (signed integer types start with `i` instead of `u`) that takes up 32 bits of space. Таблиця 3-1 показує вбудовані цілі типи в Rust. Ми можемо скористатися будь-яким з них для оголошення типу цілого числа.

<span class="caption">Таблиця 3-1: Цілі типи в Rust</span>

| Довжина                 | Знаковий | Беззнаковий |
| ----------------------- | -------- | ----------- |
| 8 бітів                 | `i8`     | `u8`        |
| 16 бітів                | `i16`    | `u16`       |
| 32 біти                 | `i32`    | `u32`       |
| 64 біти                 | `i64`    | `u64`       |
| 128 бітів               | `i128`   | `u128`      |
| Залежно від архітектури | `isize`  | `usize`     |

Кожен цілий тип є знаковим чи беззнаковим і має явно зазначений розмір. *Знаковий* і *беззнаковий* стосується того, чи може число бути від'ємним — іншими словами, чи має число знак (знакове) чи воно буде лише додатним і, відтак, може бути представлене без знаку (беззнакове). Це як запис чисел на папері: якщо знак має значення, число записується зі знаком плюс чи знаком мінус; але, якщо можна вважати, що число буде додатним, воно записується без знаку. Signed numbers are stored using [two’s complement][twos-complement]<!-- ignore
--> .

Кожен знаковий цілий тип може зберігати числа від -(2<sup>n - 1</sup>) до 2<sup>n - 1</sup> - 1 включно, де *n* - кількість біт, які він використовує. Так, `i8` може зберігати числа від -(2<sup>7</sup>) до 2<sup>7</sup> - 1, тобто від -128 до 127. Беззнакові цілі типи зберігають числа від 0 до 2<sup>n</sup> - 1, так, `u8` може зберігати числа від 0 до 2<sup>8</sup> - 1, тобто від 0 до 255.

Additionally, the `isize` and `usize` types depend on the architecture of the computer your program is running on, which is denoted in the table as “arch”: 64 bits if you’re on a 64-bit architecture and 32 bits if you’re on a 32-bit architecture.

Ви можете писати цілі літерали в будь-якій формі, вказаній у Таблиці 3-2. Зверніть увагу, що числові літерали, які можуть бути різних типів, дозволяють використовувати суфікс типу на кшталт `57u8`, для визначення типу. Числові літерали також можуть використовувати `_` як роздільник для поліпшення читання, як-от `1_000`, що позначає те саме значення, що й запис `1000`.

<span class="caption">Таблиця 3-2: Цілі літерали в Rust</span>

| Числові літерали | Приклад       |
| ---------------- | ------------- |
| Десятковий       | `98_222`      |
| Шістнадцятковий  | `0xff`        |
| Вісімковий       | `0o77`        |
| Двійковий        | `0b1111_0000` |
| Байт (лише `u8`) | `b'A'`        |

Як же зрозуміти, який тип цілого використати? Якщо ви непевні, вибір Rust за замовучанням зазвичай непоганий, а цілий тип за замовчуванням в Rust - `i32`. Основна ситуація, в якій варто використовувати `isize` та `usize` - індексація якого виду колекції.

> ##### Переповнення цілого числа
> 
> Скажімо, що у вас є змінна типу `u8`, що може мати значення між 0 та 255. If you try to change the variable to a value outside that range, such as 256, *integer overflow* will occur, which can result in one of two behaviors. When you’re compiling in debug mode, Rust includes checks for integer overflow that cause your program to *panic* at runtime if this behavior occurs. Rust uses the term *panicking* when a program exits with an error; we’ll discuss panics in more depth in the [“Unrecoverable Errors with `panic!`”][unrecoverable-errors-with-panic]<!-- ignore --> Розділу 9.
> 
> Коли ж ви компілюєте в режимі релізу за допомогою прапорця  `--release`, Rust *не* додає перевірок на переповнення, що спричинили б паніку. Натомість якщо виникає переповнення, Rust *загортає з доповненням до двох* це число. Якщо коротко, значення, більші за максимальне значення, що вміщується в тип, "загортаються" до мінімального значення, що вміщується в тип. У випадку з `u8`, значення 256 стає 0, 257 стає 1 і так далі. Програма не панікуватиме, але змінна матиме значення, що, мабуть, не відповідає вашим очікуванням. Не варто розраховувати на загортання при переповненні як на коректну поведінку, це помилка.
> 
> To explicitly handle the possibility of overflow, you can use these families of methods provided by the standard library for primitive numeric types:
> 
> * Wrap in all modes with the `wrapping_*` methods, such as `wrapping_add`.
> * Return the `None` value if there is overflow with the `checked_*` methods.
> * Return the value and a boolean indicating whether there was overflow with the `overflowing_*` methods.
> * Saturate at the value’s minimum or maximum values with the `saturating_*` methods.

#### Числа з рухомою комою

Також Rust має два примітивні типи для *чисел з рухомою комою*, тобто чисел з десятковою комою. Числа з рухомою комою в Rust - це `f32` та `f64`, які мають розмір у 32 біти та 64 біти відповідно. The default type is `f64` because on modern CPUs, it’s roughly the same speed as `f32` but is capable of more precision. Усі числа з рухомою комою знакові.

Ось приклад, що демонструє числа з рухомою комою у дії:

<span class="filename">Файл: src/main.rs</span>

```rust
{{#rustdoc_include ../listings/ch03-common-programming-concepts/no-listing-06-floating-point/src/main.rs}}
```

Числа з рухомою комою представлені відповідно до стандарту IEEE-754. Тип `f32` є числом одинарної точності, а `f64` має подвійну точність.

#### Числові операції

Rust supports the basic mathematical operations you’d expect for all the number types: addition, subtraction, multiplication, division, and remainder. Integer division truncates toward zero to the nearest integer. Наступний код демонструє, як використовувати числові операції в інструкції `let`:

<span class="filename">Файл: src/main.rs</span>

```rust
{{#rustdoc_include ../listings/ch03-common-programming-concepts/no-listing-07-numeric-operations/src/main.rs}}
```

Кожен вираз використовує математичний оператор і обчислює значення, яке прив'язується до змінної. [Appendix B][appendix_b]<!-- ignore --> contains a list of all operators that Rust provides.

#### Булівський тип

Як і в більшості інших мов програмування, булівський тип у Rust має два можливі значення: `true` ("істина") та `false` ("неправда"). Булівський тип займає 1 байт. Булівський тип у Rust позначається `bool`. Наприклад:

<span class="filename">Файл: src/main.rs</span>

```rust
{{#rustdoc_include ../listings/ch03-common-programming-concepts/no-listing-08-boolean/src/main.rs}}
```

Основний спосіб використання булівських значень - умовні вирази, такі, як вираз `if`. We’ll cover how `if` expressions work in Rust in the [“Control Flow”][control-flow]<!-- ignore --> .

#### Символьний тип

Тип`` `char `` в Rust є найпростішим алфавітним типом. Here are some examples of declaring `char` values:

<span class="filename">Файл: src/main.rs</span>

```rust
{{#rustdoc_include ../listings/ch03-common-programming-concepts/no-listing-09-char/src/main.rs}}
```

Зверніть увагу, що літерали `char` позначаються одинарними лапками, на відміну від стрічкових літералів, які послуговуються подвійними. Тип `char` в Rust має чотири байти і представляє cкалярне значення в Юнікоді, тобто може представляти значно більше, ніж просто ASCII. Літери з наголосами, китайські, японські і корейські символи, смайлики і пробіли нульової ширини є коректними значеннями для `char` у Rust. Скалярні значення Юнікода можуть бути в діапазоні від `U+0000` до `U+D7FF` і `U+E000` до `U+10FFFF` включно. Однак "символ" насправді не є концепцією Юнікода, тому ваше інтуїтивне уявлення про те, що таке "символ" може не зовсім відповідати тому, чим є `char` у Rust. We’ll discuss this topic in detail in [“Storing UTF-8 Encoded Text with Strings”][strings]<!-- ignore --> Розділу 8.

### Складені типи

*Складені типи* дозволяють об'єднувати багато значень в один тип. Rust має два базових складених типи: кортежі та масиви.

#### Тип кортеж

A *tuple* is a general way of grouping together a number of values with a variety of types into one compound type. Tuples have a fixed length: once declared, they cannot grow or shrink in size.

Кортеж утворюється списком значень, розділених комами, в дужках. Кожна позиція в кортежі має тип, і типи різних значень у кортежі не обов'язково мають збігатися. Ми додали необов'язкову анотацію типу у цьому прикладі:

<span class="filename">Файл: src/main.rs</span>

```rust
{{#rustdoc_include ../listings/ch03-common-programming-concepts/no-listing-10-tuples/src/main.rs}}
```

The variable `tup` binds to the entire tuple because a tuple is considered a single compound element. Щоб отримати окремі значення з кортежу, можна скористатися зіставлянням з шаблоном, щоб деструктуризувати значення кортежу, на кшталт цього:

<span class="filename">Файл: src/main.rs</span>

```rust
{{#rustdoc_include ../listings/ch03-common-programming-concepts/no-listing-11-destructuring-tuples/src/main.rs}}
```

Ця програма спершу створює кортеж і зв'язує його зі змінною `tup`. Далі вона використовує шаблон з `let`, щоб перетворити `tup` на три окремі змінні: `x`, `y` і `z`. This is called *destructuring* because it breaks the single tuple into three parts. І врешті програма виводить значення `y`, тобто `6.4`.

Ми також можемо отримати доступ до елементу кортежу напряму за допомогою точки (`.`), за якою іде індекс значення, яке нам треба отримати. Наприклад:

<span class="filename">Файл: src/main.rs</span>

```rust
{{#rustdoc_include ../listings/ch03-common-programming-concepts/no-listing-12-tuple-indexing/src/main.rs}}
```

Ця програма створює кортеж `x`, а потім створює нові змінні для кожного елементу за допомогою їхніх індексів. Як і в більшості мов програмування, перший індекс в кортежі - 0.

Кортеж без значень має особливу назву - *одиничний тип*. Це значення і відповідний тип обидва записуються як `()` і представляють порожнє значення чи порожній тип результату. Вирази неявно повертають одиничний тип, якщо вони не повертають жодного іншого значення.

#### Тип Масив

Інший спосіб організувати колекцію з багатьох значень - це *масив*. На відміну від кортежу, всі елементи масиву мусять мати один тип. На відміну від масивів у деяких інших мовах, масиви в Rust мають фіксовану довжину.

We write the values in an array as a comma-separated list inside square brackets:

<span class="filename">Файл: src/main.rs</span>

```rust
{{#rustdoc_include ../listings/ch03-common-programming-concepts/no-listing-13-arrays/src/main.rs}}
```

Arrays are useful when you want your data allocated on the stack rather than the heap (we will discuss the stack and the heap more in [Chapter 4][stack-and-heap]<!-- ignore -->), чи коли ви хочете бути певним, що завжди маєте фіксовану кількість елементів. Втім, масиви не такі гнучкі, як вектори. A *vector* is a similar collection type provided by the standard library that *is* allowed to grow or shrink in size. If you’re unsure whether to use an array or a vector, chances are you should use a vector. [Chapter 8][vectors]<!-- ignore --> розповідає про вектори детальніше.

Разом із тим, масиви корисніші, коли ви знаєте, що кількість елементів не треба буде змінювати. Наприклад, коли ви використовуєте імена місяців у програмі, швидше за все ви використаєте масив, а не вектор, бо ви знаєте, що він завжди складатиметься з 12 елементів:

```rust
let months = ["January", "February", "March", "April", "May", "June", "July",
              "August", "September", "October", "November", "December"];
```

You write an array’s type using square brackets with the type of each element, a semicolon, and then the number of elements in the array, like so:

```rust
let a: [i32; 5] = [1, 2, 3, 4, 5];
```

Тут `i32` є типом кожного елементу. Після крапки з комою, число `5` позначає, що масив містить п'ять елементів.

You can also initialize an array to contain the same value for each element by specifying the initial value, followed by a semicolon, and then the length of the array in square brackets, as shown here:

```rust
let a = [3; 5];
```

Масив, що зветься `a`, міститиме `5` елементів, що початково матимуть значення `3`. Це - те саме, що написати `let a = [3, 3, 3, 3, 3];` але стисліше.

##### Доступ до елементів масиву

Масив - це єдиний блок пам'яті відомого, фіксованого розміру, що може бути виділений у стеку. Ви можете працювати з елементами масиву за допомогою індексів, ось так:

<span class="filename">Файл: src/main.rs</span>

```rust
{{#rustdoc_include ../listings/ch03-common-programming-concepts/no-listing-14-array-indexing/src/main.rs}}
```

In this example, the variable named `first` will get the value `1` because that is the value at index `[0]` in the array. The variable named `second` will get the value `2` from index `[1]` in the array.

##### Некоректний доступ до елементів масиву

Подивімося, що станеться, якщо ви спробуєте дістатися до елемента масиву, що знаходиться за його кінцем. Скажімо, ви запустите цей код, схожий на гру-здогадайку з Розділу 2, щоб отримати індекс масиву від користувача:

<span class="filename">Файл: src/main.rs</span>

```rust,ignore,panics
{{#rustdoc_include ../listings/ch03-common-programming-concepts/no-listing-15-invalid-array-access/src/main.rs}}
```

Цей код успішно компілюється. If you run this code using `cargo run` and enter `0`, `1`, `2`, `3`, or `4`, the program will print out the corresponding value at that index in the array. If you instead enter a number past the end of the array, such as `10`, you’ll see output like this:

<!-- manual-regeneration
cd listings/ch03-common-programming-concepts/no-listing-15-invalid-array-access
cargo run
10
-->

```console
thread 'main' panicked at 'index out of bounds: the len is 5 but the index is 10', src/main.rs:19:19
note: run with `RUST_BACKTRACE=1` environment variable to display a backtrace
```

Програма завершилася помилкою *часу виконання* у той момент, коли використала некоректне значення при застосуванні індексу. Програма вийшла з повідомленням про помилку і не виконала останню інструкцію `println!`. Коли ви намагаєтеся отримати доступ до елементу масиву, Rust перевіряє, чи зазначений індекс менший за довжину масиву. Якщо індекс більший чи дорівнює довжині, Rust панікує. Ця перевірка здійснюється під час виконання, особливо в цьому випадку, бо компілятор не має жодної можливості знати, яке значення введе користувач, коли код буде запущено.

Це приклад безпеки роботи з пам'яттю Rust у дії. У багатьох мовах низького рівня такої перевірки не відбувається, і коли ви задаєте некоректний індекс, може відбутися доступ до некоректної пам'яті. Rust захищає вас від такої помилки, одразу перериваючи роботу програми замість того, щоб дозволити некоректний доступ і продовжити роботу. Розділ 9 розповідає більше про обробку помилок у Rust і як ви можете писати читаний, безпечний код що не панікує і не дозволяє некоректний доступ до пам'яті.
ch02-00-guessing-game-tutorial.html#comparing-the-guess-to-the-secret-number

[twos-complement]: https://en.wikipedia.org/wiki/Two%27s_complement
[control-flow]: ch03-05-control-flow.html#control-flow
[strings]: ch08-02-strings.html#storing-utf-8-encoded-text-with-strings
[stack-and-heap]: ch04-01-what-is-ownership.html#the-stack-and-the-heap
[vectors]: ch08-01-vectors.html
[unrecoverable-errors-with-panic]: ch09-01-unrecoverable-errors-with-panic.html
[appendix_b]: appendix-02-operators.md
