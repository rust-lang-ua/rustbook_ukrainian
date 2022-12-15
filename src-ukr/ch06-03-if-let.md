## Лаконічний контроль виконання конструкцією `if let`

Конструкція `if let` дозволяє вам комбінувати `if` та `let` менш багатослівно, щоб обробляти значення, що відповідають одному шаблону, і ігнорувати інші. Розглянемо програму у Блоці коду 6-6, що працює зі значенням `Option<u8>` у змінній `config_max`, але хоче виконувати код лише коли значення є варіантом `Some`.

```rust
{{#rustdoc_include ../listings/ch06-enums-and-pattern-matching/listing-06-06/src/main.rs:here}}
```


<span class="caption">Listing 6-6: A `match` that only cares about executing code when the value is `Some`</span>

Якщо значення є `Some`, ми виводимо значення у варіанті `Some`, зв'язавши у шаблоні це значення зі змінною `max`. Ми не хочемо нічого робити зі значенням `None`. Щоб задовольнити вираз `match`, нам доводиться додати `_ =>()` після обробки лише одного варіанту, що є набридливо надлишковим.

Натомість ми можемо записати це коротше за допомогою `if let`. Наступний код робить те саме, що й `match` з Блоку коду 6-6:

```rust
{{#rustdoc_include ../listings/ch06-enums-and-pattern-matching/no-listing-12-if-let/src/main.rs:here}}
```

Конструкція `if let` бере шаблон і вираз, розділені знаком рівності. Вона працює так само як і `match`, де вираз стоїть після `match`, а шаблон є його першим рукавом. У цьому випадку шаблоном буде `Some(max)`, і `max` зв'язується зі значенням всередині `Some`. Тепер ми можемо використати `max` у тілі блоку `if let` так само, як ми використали `max` у відповідному рукаві `match`. Код у блоці `if let` не буде виконано, якщо значення не відповідає шаблону.

Використання `if let` означає, що вам треба менше друкувати, менше ставити відступів і писати менше зайвого коду. Разом з тим, ми втрачаємо перевірку на вичерпність, до якої зобов'язує `match`. Вибір між `match` та `if let` залежить від того, що ви робите у конкретній ситуації та чи лаконічність варта втрати перевірки на вичерпність.

In other words, you can think of `if let` as syntax sugar for a `match` that runs code when the value matches one pattern and then ignores all other values.

У `if let` можна також додати `else`. Блок, що іде після `else` - це той самий блок, що був би у випадку `_` у виразу `match`, еквівалентному нашому `if let` та `else`. Згадаємо визначення енума `Coin` у Блоці коду 6-4, де варіант `Quarter` також включав значення `UsState`. Якби ми захотіли полічити усе, крім четвертаків, і водночас виводити штат з четвертаків, ми могли б зробити це за допомогою десь такого виразу `match`:

```rust
{{#rustdoc_include ../listings/ch06-enums-and-pattern-matching/no-listing-13-count-and-announce-match/src/main.rs:here}}
```

Або ж ми могли б скористатися виразом `if let` та `else` ось таким чином:

```rust
{{#rustdoc_include ../listings/ch06-enums-and-pattern-matching/no-listing-14-count-and-announce-if-let-else/src/main.rs:here}}
```

If you have a situation in which your program has logic that is too verbose to express using a `match`, remember that `if let` is in your Rust toolbox as well.

## Підсумок

Ми щойно розібрали, як використовувати енуми для створення власних типів, які можуть набувати одне з множини перелічених значень. Ми показали, як тип `Option<T>` зі стандартної бібліотеки допомагає використовувати систему типів для уникання помилок. Коли значення енума мають дані всередині, можна скористатися `match` чи `if let`, щоб витягти та використати ці значення, залежно від того, скільки різних варіантів вам треба обробити.

Ваші програми Rust тепер можуть виражати концепції з проблемної області за допомогою структур та енумів. Створення власних типів для використання у вашому API гарантує безпеку типів: компілятор забезпечить, що ваші функції отримають лише значення тих типів, на які ці функції очікують.

In order to provide a well-organized API to your users that is straightforward to use and only exposes exactly what your users will need, let’s now turn to Rust’s modules.
