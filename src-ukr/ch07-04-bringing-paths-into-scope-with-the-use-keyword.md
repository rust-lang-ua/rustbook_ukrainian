## Підключення шляхів до області видимості за допомогою ключового слова `use`

Необхідність переписувати шляхи для виклику функцій може здатися незручною та повторюваною. В Лістингу 7-7 незалежно від того, чи ми вказували абсолютний чи відносний шлях до функції `add_to_waitlist`, для того щоб її викликати ми кожного разу мали також вказувати `front_of_house` та `hosting`. На щастя, існує спосіб спростити цей процес: достатньо один раз створити ярлик (shortcut) для шляху за допомогою ключового слова `use` і потім використовувати коротке імʼя будь-де в області видимості.

In Listing 7-11, we bring the `crate::front_of_house::hosting` module into the scope of the `eat_at_restaurant` function so we only have to specify `hosting::add_to_waitlist` to call the `add_to_waitlist` function in `eat_at_restaurant`.

<span class="filename">Файл: src/lib.rs</span>

```rust,noplayground,test_harness
{{#rustdoc_include ../listings/ch07-managing-growing-projects/listing-07-11/src/lib.rs}}
```


<span class="caption">Listing 7-11: Bringing a module into scope with `use`</span>

Додання `use` та шляху до області видимості схоже на створення символічного посилання (symbolic link) у файловій системі. При додаванні `use crate::front_of_house::hosting` в корені крейта, `hosting` стає коректним імʼям в цій області видимості, так як би модуль ``hosting` був визначений в корені крейта. Шляхи, додані до області видимості за допомогою `use`, також перевіряються на приватність, як і будь-які інші.

Зауважте, що `use` лише створює ярлик для конкретної області видимості, в якій знаходиться цей самий `use`. Лістинг 7-12 переносить функцію `eat_at_restaurant` до нового дочірнього модуля `customer`, що має відмінну від `use` область видимості, а отже, тіло фінкції зкомпільовано не буде:

<span class="filename">Файл: src/lib.rs</span>

```rust,noplayground,test_harness,does_not_compile,ignore
{{#rustdoc_include ../listings/ch07-managing-growing-projects/listing-07-12/src/lib.rs}}
```


<span class="caption">Listing 7-12: A `use` statement only applies in the scope it’s in</span>

The compiler error shows that the shortcut no longer applies within the `customer` module:

```console
{{#include ../listings/ch07-managing-growing-projects/listing-07-12/output.txt}}
```

Зверніть увагу також на попередження компілятора, що `use` не використовується у власній області видимості! Для вирішення цієї проблеми треба перемістити `use` до модуля `customer`, або послатися на його ярлик у батьківському модулі за допомогою `super::hosting` всередині дочірнього модуля`customer`.

### Створення ідіоматичних шляхів `use`

In Listing 7-11, you might have wondered why we specified `use
crate::front_of_house::hosting` and then called `hosting::add_to_waitlist` in `eat_at_restaurant` rather than specifying the `use` path all the way out to the `add_to_waitlist` function to achieve the same result, as in Listing 7-13.

<span class="filename">Файл: src/lib.rs</span>

```rust,noplayground,test_harness
{{#rustdoc_include ../listings/ch07-managing-growing-projects/listing-07-13/src/lib.rs}}
```


<span class="caption">Listing 7-13: Bringing the `add_to_waitlist` function into scope with `use`, which is unidiomatic</span>

Хоча Лістинги 7-11 та 7-13 і виконують одну й ту саму задачу, Лістинг 7-11 є ідіоматичним способом додавання функції до області видимості за допомогою `use`. Щоб додати батьківський модуль функції до області видимості з `use` треба його вказати при виклику функції. Вказання батьківського модуля при виклику функції явно показує, що функція не оголошена локально, але разом з тим це зводить до мінімуму необхідність повторень повного шляху. З коду в Лістингу 7-13 не ясно, де саме визначено `add_to_waitlist`.

З іншого боку при додаванні структур, переліків та інших елементів за допомогою `use`, вказання повного шляху є ідіоматичним. Лістинг 7-14 демонструє ідіоматичний спосіб для додавання стандартної структури з бібліотеки `HashMap`\` до області видимості бінарного крейту.

<span class="filename">Файл: src/main.rs</span>

```rust
{{#rustdoc_include ../listings/ch07-managing-growing-projects/listing-07-14/src/main.rs}}
```


<span class="caption">Listing 7-14: Bringing `HashMap` into scope in an idiomatic way</span>

There’s no strong reason behind this idiom: it’s just the convention that has emerged, and folks have gotten used to reading and writing Rust code this way.

Винятком з цієї ідіоми є випадок, коли треба підключити два елементи з однаковими іменами до області видимості з оператором `use`, оскільки Rust не дозволяє зробити це. Лістинг 7-15 демонструє як підключити до області видимості два типи `Result`, що мають однакове імʼя, але різні батьківські модулі, та як до них звертатися.

<span class="filename">Файл: src/lib.rs</span>

```rust,noplayground
{{#rustdoc_include ../listings/ch07-managing-growing-projects/listing-07-15/src/lib.rs:here}}
```


<span class="caption">Listing 7-15: Bringing two types with the same name into the same scope requires using their parent modules.</span>

Як ви можете бачити, використання батьківських модулів розрізняє дви типа `Result`. Якщо б натомість ми вказали `use std::fmt::Result` та `use std::io::Result`, ми б мали два типи `Result` в одній області видимості та Rust не знав би, який з них ми маємо на увазі, пишучи `Result`.

### Впровадження нових імен за допомогою ключового слова `as`

Існує також інше рішення проблеми використання двох типів з одним імʼя в одній області видимості з `use`: після шляху можна вказати `as` та нове локальне імʼя, або *аліас* для даного типу. Лістинг 7-16 показує інший спосіб написання коду з Лістинга 7-15, перейменувавши один з двох типів `Result` за допомогою `as`.

<span class="filename">Файл: src/lib.rs</span>

```rust,noplayground
{{#rustdoc_include ../listings/ch07-managing-growing-projects/listing-07-16/src/lib.rs:here}}
```


<span class="caption">Listing 7-16: Renaming a type when it’s brought into scope with the `as` keyword</span>

У другому операторі `use`ми вказали нове імʼя `IoResult`для типу ``std::io::Result`, що не конфліктуватиме з типом `Result` з `std::fmt`, що ми ойго також додали до області видимості. Підходи з лістингів 7-15 та 7-16 вважаються ідіоматичними. Отже, вибір за вами!

### Реекспорт імен із `pub use`

При внесенні імені до області видимості із ключовим словом `use`, імʼя, доступне в новій області видимості, є приватним. Аби код міг посилатися на це імʼя так, ніби воно визначене в його області видимості, ми можемо комбінувати `pub` та `use`. Ця техніка називається *re-exporting*. тому що ми не лише додаємо елемент до області видимості, а ще й робимо його доступним для підключення в інші області видимості.

Listing 7-17 shows the code in Listing 7-11 with `use` in the root module changed to `pub use`.

<span class="filename">Файл: src/lib.rs</span>

```rust,noplayground,test_harness
{{#rustdoc_include ../listings/ch07-managing-growing-projects/listing-07-17/src/lib.rs}}
```


<span class="caption">Listing 7-17: Making a name available for any code to use from a new scope with `pub use`</span>

До цієї заміни зовнішній код повинен був викликати функцію `add_to_waitlist`, використовуючи шлях `restaurant::front_of_house::hosting::add_to_waitlist()`. Тепер, коли використання `pub
use` дозволило реекспортувати модуль `hosting` з кореневого модуля, зовнішній код може натомість використовувати шлях `restaurant::hosting::add_to_waitlist()`.

Реекспорт є корисним, коли внутрішня структура коду відрізняється від того, як програмісти, що викликають ваш код, думають про предметну область. Наприклад, в нашій ресторанній метафорі люди, що керують рестораном, сприймають його як внутрішню кухню та зал В той час як відвідувачі ресторану, можливо, не сприймають ресторан в таких само термінах. Із `pub use` ми можемо писати код у вигляді однієї структури, проте виставляти його назовні у вигляді іншої. Завдяки цьому наша бібліотека лишається добре організованою для програмістів, які будуть з нею працювати. Ми також розглянемо інший приклад використання `pub use` і як це впливає на вашу документацію крейту в частині [“Експорт зручного публічного API із `pub use`”][ch14-pub-use]<!-- ignore --> розділу 14.

### Використання зовнішніх пакетів

У Розділі 2 ми написали гру у вгадування чисел, яка використовувала зовнішній пакет під назвою `rand` для отримання випадкових чисел. Для використання `rand` в нашому проекті ми додали наступний рядок до *Cargo.toml*:

<!-- When updating the version of `rand` used, also update the version of
`rand` used in these files so they all match:
* ch02-00-guessing-game-tutorial.md
* ch14-03-cargo-workspaces.md
-->

<span class="filename">Файл: Cargo.toml</span>

```toml
{{#include ../listings/ch02-guessing-game-tutorial/listing-02-02/Cargo.toml:9:}}
```

Adding `rand` as a dependency in *Cargo.toml* tells Cargo to download the `rand` package and any dependencies from [crates.io](https://crates.io/) and make `rand` available to our project.

Потім, для того щоб додати `rand` до області видимості нашого пакету, ми додали рядок `use`, що починався з імені крейту `rand` та перелічили елементи, які ми хочемо додати до області видимості. Згадайте, що в секції [“Генерація випадкового числа”][rand]<!-- ignore --> розділу 2 ми додали трейт `Rng` до області видимості і викликали функцію `rand::thread_rng`:

```rust,ignore
{{#rustdoc_include ../listings/ch02-guessing-game-tutorial/listing-02-03/src/main.rs:ch07-04}}
```

Members of the Rust community have made many packages available at [crates.io](https://crates.io/), and pulling any of them into your package involves these same steps: listing them in your package’s *Cargo.toml* file and using `use` to bring items from their crates into scope.

Зверніть увагу, що стандартна бібліотека `std` є також крейтом, щщо є зовнішнім по відношенню до нашого пакету. Оскільки стандартна бібліотека поставляється в комплекті з мовою Rust, нам не портібно змінювати *Cargo.toml* для додання `std`. Але нам потрібно вказати її за допомогою `use` для того щоб додати її елементи до області видимості нашого пакету. Наприклад, для `HashMap` ми б використовували такий рядок:

```rust
use std::collections::HashMap;
```

This is an absolute path starting with `std`, the name of the standard library crate.

### Використаня вкладенних шляхів для зменшення величезних переліків `use`

Якщо нам треба використовувати багато елементів, визначених в тому самому крейті або модулі, вказання кожного з них на окремому рядку займає багато вертикального простору в файлах. Наприклад, ці два оголошення `use` ми використовували у грі вгадування чисел в Лістингу 2-4 для додання до області видимості елементів з `std`:

<span class="filename">Файл: src/main.rs</span>

```rust,ignore
{{#rustdoc_include ../listings/ch07-managing-growing-projects/no-listing-01-use-std-unnested/src/main.rs:here}}
```

Натомість, ми можемо використовувати вкладені шляхи для того щоб додати ці елементи до області видимості лише одним рядком. Для цього ми вказуємо спільну частину шляху, за якою йдуть дві двокрапки, а потім фігурні дужки навколо переліку частин шляхів, що відрізняються, як показано в Лістінгу 7-18.

<span class="filename">Файл: src/main.rs</span>

```rust,ignore
{{#rustdoc_include ../listings/ch07-managing-growing-projects/listing-07-18/src/main.rs:here}}
```


<span class="caption">Listing 7-18: Specifying a nested path to bring multiple items with the same prefix into scope</span>

In bigger programs, bringing many items into scope from the same crate or module using nested paths can reduce the number of separate `use` statements needed by a lot!

Ми можемо використовувати вкладені шляхи будь-якого рівня вкладеності, що є корисним при комбінуванні двох виразів `use`, що мають спільну частину шляху. Наприклад, Лістинг 7-19 демонструє два оператора `use`: один додає до області видимості `std::io` і один, що додає `std::io::Write`.

<span class="filename">Файл: src/lib.rs</span>

```rust,noplayground
{{#rustdoc_include ../listings/ch07-managing-growing-projects/listing-07-19/src/lib.rs
```


<span class="caption">Listing 7-19: Two `use` statements where one is a subpath of the other</span>

Спільною частиною цих двох шляхів є `std::io`, і це ж є повним шляхом першого. Для обʼєднання цих двох шляхів в один оператор `use` ми можемо використати ключове слово `self` у вкладеному шляху, як показано в Лістингу 7-20.

<span class="filename">Файл: src/lib.rs</span>

```rust,noplayground
{{#rustdoc_include ../listings/ch07-managing-growing-projects/listing-07-20/src/lib.rs}}
```


<span class="caption">Listing 7-20: Combining the paths in Listing 7-19 into one `use` statement</span>

Цей рядок додає `std::io` та `std::io::Write` до області видимості.

### Глобальний оператор (*)

If we want to bring *all* public items defined in a path into scope, we can specify that path followed by the `*` glob operator:

```rust
use std::collections::*;
```

Цей оператор `use` додає до області видимості всі публічні елементи, визначені в `std::collections`. Будьте обережні, використовуючи глобальний оператор! Це може ускладнити сприйняття коду, оскільки стає важче визначити, які імена є в області видимості і де саме було визначено певне імʼя, що використовується у вашій програмі.

Лобальний оператор часто використовується при тестуванні для включення до області видимості всіх елементів з модуля `tests`. Ми поговоримо про це пізніше у секції [“Як писати тести”][writing-tests]<!-- ignore --> розділу 11. Глобальний оператор також інколи використовується як частина патерну Прелюдія (prelude): див. [документацію по стандартній бібліотеці](../std/prelude/index.html#other-preludes)<!-- ignore -->
для отримання додаткової інформації по цьому патерну.

[ch14-pub-use]: ch14-02-publishing-to-crates-io.html#exporting-a-convenient-public-api-with-pub-use
[rand]: ch02-00-guessing-game-tutorial.html#generating-a-random-number
[writing-tests]: ch11-01-writing-tests.html#how-to-write-tests