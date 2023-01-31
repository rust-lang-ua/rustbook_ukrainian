## Синтаксис методів

*Методи* подібні до функцій: вони проголошуються ключовим словом `fn` і іменем, можуть мати параметри та повертати значення, і містять код, що виконується, коли їх викликають з іншого місця. Unlike functions, methods are defined within the context of a struct (or an enum or a trait object, which we cover in [Chapter 6][enums]<!-- ignore --> and [Chapter 17][trait-objects]<!-- ignore -->, respectively), and their first parameter is always `self`, which represents the instance of the struct the method is being called on.

### Визначення методів

Let’s change the `area` function that has a `Rectangle` instance as a parameter and instead make an `area` method defined on the `Rectangle` struct, as shown in Listing 5-13.

<span class="filename">Файл: src/main.rs</span>

```rust
{{#rustdoc_include ../listings/ch05-using-structs-to-structure-related-data/listing-05-13/src/main.rs}}
```


<span class="caption">Listing 5-13: Defining an `area` method on the `Rectangle` struct</span>

Щоб визначити функцію в контексті `Rectangle`, ми починаємо блок `impl` (від implementation, "реалізація") для `Rectangle`. Все в цьому блоці `impl` буде пов'язано з типом `Rectangle`. Потім ми переносимо функцію `area` до фігурних дужок після `impl` і замінюємо перший (а в цьому випадку єдиний) параметр на `self` у сигнатурі та повсюди в тілі. У `main`, де ми викликали функцію `area` і передавали аргументом `rect1`, тепер використаємо *синтаксис виклику метода*, щоб викликати метод `area` нашого екземпляра `Rectangle`. Синтаксис виклику методу записується після екземпляру: ми додаємо крапку, за якою - ім'я методу, дужки, і параметри, якщо такі є.

У сигнатурі `area` ми використовуємо `&self` замість `rectangle: &Rectangle`. `&self` є насправді скороченням для `self: &Self`. Усередині блоку `impl` тип `Self` є псевдонімом для типу, для якого призначено цей блок `impl`. Методи мусять мати перший параметр на ім'я `self` типу `Self`, тому Rust дозволяє вам скоротити це до лише імені `self` на місці першого параметра. Note that we still need to use the `&` in front of the `self` shorthand to indicate that this method borrows the `Self` instance, just as we did in `rectangle: &Rectangle`. Methods can take ownership of `self`, borrow `self` immutably, as we’ve done here, or borrow `self` mutably, just as they can any other parameter.

We chose `&self` here for the same reason we used `&Rectangle` in the function version: we don’t want to take ownership, and we just want to read the data in the struct, not write to it. If we wanted to change the instance that we’ve called the method on as part of what the method does, we’d use `&mut self` as the first parameter. Having a method that takes ownership of the instance by using just `self` as the first parameter is rare; this technique is usually used when the method transforms `self` into something else and you want to prevent the caller from using the original instance after the transformation.

The main reason for using methods instead of functions, in addition to providing method syntax and not having to repeat the type of `self` in every method’s signature, is for organization. We’ve put all the things we can do with an instance of a type in one `impl` block rather than making future users of our code search for capabilities of `Rectangle` in various places in the library we provide.

Зверніть увагу, що ми можемо вирішити назвати метод так само як зветься одне з полів структури. For example, we can define a method on `Rectangle` that is also named `width`:

<span class="filename">Файл: src/main.rs</span>

```rust
{{#rustdoc_include ../listings/ch05-using-structs-to-structure-related-data/no-listing-06-method-field-interaction/src/main.rs:here}}
```

Here, we’re choosing to make the `width` method return `true` if the value in the instance’s `width` field is greater than `0` and `false` if the value is `0`: we can use a field within a method of the same name for any purpose. In `main`, when we follow `rect1.width` with parentheses, Rust knows we mean the method `width`. When we don’t use parentheses, Rust knows we mean the field `width`.

Often, but not always, when we give a method the same name as a field we want it to only return the value in the field and do nothing else. Methods like this are called *getters*, and Rust does not implement them automatically for struct fields as some other languages do. Getters are useful because you can make the field private but the method public, and thus enable read-only access to that field as part of the type’s public API. We will discuss what public and private are and how to designate a field or method as public or private in [Chapter 7][public]<!-- ignore -->.

> ### А де ж оператор `->`?
> 
> У C та C++ використовуються два різні оператори для виклику методів: `.`, якщо метод викликається для об'єкта безпосередньо, і `->`, якщо ви викликаєте метод для вказівника на об'єкт і спершу вказівник слід розіменувати. Іншими словами, якщо `object` - це вказівник, то `object->something()` робить те саме, що й `(*object).something()`.
> 
> Rust не має еквівалента оператора`->`; натомість, Rust має особливість, що зветься *автоматичне посилання і розіменування* (automatic referencing and dereferencing). Виклик методів - це одне з небагатьох місць у Rust з такою поведінкою.
> 
> Ось як це працює: коли ви викликаєте метод з  `object.something()`, Rust автоматично додає `&`, `&mut`, або `*`, щоб `object` відповідав сигнатурі методу. Іншими словами, наступними вирази означають одне й те саме:
> 
> <!-- CAN'T EXTRACT SEE BUG https://github.com/rust-lang/mdBook/issues/1127 -->
> 
> ```rust
> # #[derive(Debug,Copy,Clone)]
> # struct Point {
> #     x: f64,
> #     y: f64,
> # }
> #
> # impl Point {
> #    fn distance(&self, other: &Point) -> f64 {
> #        let x_squared = f64::powi(other.x - self.x, 2);
> #        let y_squared = f64::powi(other.y - self.y, 2);
> #
> #        f64::sqrt(x_squared + y_squared)
> #    }
> # }
> # let p1 = Point { x: 0.0, y: 0.0 };
> # let p2 = Point { x: 5.0, y: 6.5 };
> p1.distance(&p2);
> (&p1).distance(&p2);
> ```
> 
> Але перший вираз є значно яснішим. Ці автоматичні посилання працюють, бо методи мають чітко заданого отримувача - тип  `self`. Знаючи отримувача і назву метода, Rust може однозначно з'ясувати, чи цей метод для читання  (`&self`), змін (`&mut self`) чи поглинання (`self`). Те, що Rust робить позичання неявним для отримувача метода є суттєвою частиною того, що робить володіння ергономічним на практиці.

### Методи з більшою кількістю параметрів

Попрактикуймося використовувати методи, створивши другий метод для структури `Rectangle`. This time we want an instance of `Rectangle` to take another instance of `Rectangle` and return `true` if the second `Rectangle` can fit completely within `self` (the first `Rectangle`); otherwise, it should return `false`. That is, once we’ve defined the `can_hold` method, we want to be able to write the program shown in Listing 5-14.

<span class="filename">Файл: src/main.rs</span>

```rust,ignore
{{#rustdoc_include ../listings/ch05-using-structs-to-structure-related-data/listing-05-14/src/main.rs}}
```


<span class="caption">Listing 5-14: Using the as-yet-unwritten `can_hold` method</span>

The expected output would look like the following because both dimensions of `rect2` are smaller than the dimensions of `rect1`, but `rect3` is wider than `rect1`:

```text
Can rect1 hold rect2? true
Can rect1 hold rect3? false
```

Ми знаємо, що хочемо визначити метод, тож він буде написаний у блоці `impl 
Rectangle`. Метод буде зватися `can_hold`, і буде приймати параметром немутабельне позичання іншого `Rectangle`. Ми можемо зрозуміти, якого типу буде параметр, подивившися на код, що викликає метод: `rect1.can_hold(&rect2)` передає `&rect2`\`, тобто немутабельно позичає `rect2`, екземпляр `Rectangle`. Це зрозуміло, бо нам треба лише читати `rect2` (а не писати, бо тоді б було потрібне мутабельне позичання), і ми хочемо, щоб `main` залишав собі володіння `rect2`, щоб його можна було використовувати після виклику методі `can_hold`. The return value of `can_hold` will be a Boolean, and the implementation will check whether the width and height of `self` are greater than the width and height of the other `Rectangle`, respectively. Додамо метод `can_hold` до блоку `impl` з Блоку коду 5-13, як показано в Блоці коду 5-15.

<span class="filename">Файл: src/main.rs</span>

```rust
{{#rustdoc_include ../listings/ch05-using-structs-to-structure-related-data/listing-05-15/src/main.rs:here}}
```


<span class="caption">Listing 5-15: Implementing the `can_hold` method on `Rectangle` that takes another `Rectangle` instance as a parameter</span>

Коли ми запустимо цей код з функції `main` у Блоці коду 5-14, ми отримаємо вивід, який хотіли. Методи можуть приймати багато параметрів, які ми додаємо до сигнатури після параметру `self`, і ці параметри працюють так само як у функціях.

### Асоційовані функції

Усі функції, визначені в блоці `impl`, звуться *асоційованими функціями*, бо вони асоційовані з типом, названим після `impl`. Ми можемо визначити асоційовані функції, що не мають першим параметром `self` (і відтак не є методами), і вони не потребують екземпляра типа, щоб із ним працювати. Ми вже користалися такою асоційованою функцією, а саме функцією `String::from`, визначеною на типі `String`.

Асоційовані функції, що не є методами, часто використовуються як конструктори, що повертають новий екземпляр структури. Вони часто називаються `new`, але `new` не є спеціальним ім'ям і не вбудовано в мову. Наприклад, ми можемо написати асоційовану функцію `square`, що матиме один параметр розміру і використовуватиме його і як ширину, і як висоту, щоб створити таким чином квадратний `Rectangle`, не вказуючи одне й те саме значення двічі:

<span class="filename">Файл: src/main.rs</span>

```rust
{{#rustdoc_include ../listings/ch05-using-structs-to-structure-related-data/no-listing-03-associated-functions/src/main.rs:here}}
```

The `Self` keywords in the return type and in the body of the function are aliases for the type that appears after the `impl` keyword, which in this case is `Rectangle`.

Щоб викликати асоційовану функцію, ми використовуємо запис `::` з іменем структури, наприклад `let sq = Rectangle::square(3);`. Ця функція включена до простору імен структури: запис `::` використовується і для асоційованих функцій, і для просторів імен, створених модулями. We’ll discuss modules in [Chapter 7][modules]<!-- ignore -->.

### Кілька однакових блоків `impl`

Кожна структура може мати кілька блоків `impl`. For example, Listing 5-15 is equivalent to the code shown in Listing 5-16, which has each method in its own `impl` block.

```rust
{{#rustdoc_include ../listings/ch05-using-structs-to-structure-related-data/listing-05-16/src/main.rs:here}}
```


<span class="caption">Listing 5-16: Rewriting Listing 5-15 using multiple `impl` blocks</span>

Тут немає підстав розділяти ці методи у декілька блоків `impl`, але це коректний синтаксис. Ми побачимо випадок, де кілька блоків `impl` можуть бути корисні, у Розділі 10, де ми поговоримо про узагальнені типи і трейти.

## Підсумок

Структури дозволяють вам створювати власні типи, що мають значення для предметної області програми. Використовуючи структури, ми можемо зберігати пов’язані між собою фрагменти даних разом і давати ім'я кожному фрагменту, щоб зробити наш код зрозумілим. У блоках `impl` ви можете визначити функції, асоційовані з вашим типом, а методи - це різновид асоційованих функцій, що дозволяють визначити поведінку, яку мають екземпляри ваших структур.

But structs aren’t the only way you can create custom types: let’s turn to Rust’s enum feature to add another tool to your toolbox.

[enums]: ch06-00-enums.html
[trait-objects]: ch17-02-trait-objects.md
[public]: ch07-03-paths-for-referring-to-an-item-in-the-module-tree.html#exposing-paths-with-the-pub-keyword
[modules]: ch07-02-defining-modules-to-control-scope-and-privacy.html
