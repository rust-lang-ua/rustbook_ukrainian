## Що таке володіння?

Основна особливість Rust - це *володіння*. Хоча її досить легко пояснити, вона
має глибокі наслідки для усієї мови.

Усі програми мають управляти своїм використанням пам'яті комп'ютера під час 
роботи. Деякі мови мають збирача сміття, який постійно шукає пам'ять, що її вже
не використовують, під час роботи програми; в інших мовах програміст має явно
виділяти і звільняти пам'ять. Rust використовує третій підхід: пам'ять 
управляється системою володіння з набором правил, які компілятор перевіряє під 
час компіляції. Під час виконання володіння не додає жодних додаткових витрат.

Оскільки володіння - нова концепція для багатьох програмістів, потрібен деякий
час, щоб звикнути до нього. Добра новина - що досвідченішим ви ставатимете в
Rust і правилах системи володіння, тим здатнішим до створення безпечного і
ефективного коду ви ставатимете. Так тримати!

Коли ви зрозумієте володіння, ви матимете стійку основу для розуміння 
особливостей, що роблять Rust унікальною мовою. В цьому розділі, ви вивчете 
володіння, працюючи з прикладами, що концентруються на добре відомих структурах
даних: рядках.

<!-- PROD: START BOX -->

> ### Стек і купа

> У багатьох мовах програмування програміст нечасто має думати про стек і купу.
> Але в системних мовах, таких, як Rust, розташування значення в стеку чи в купі
> більше впливає на поведінку програми і вибір, який ми робимо. Ви пояснимо те,
> як стек і купа впливають на володіння пізніше у цьому розділі, а тут даємо
> коротке попедеднє пояснення.
>
> Стек і купа - частини пам'яті, до яких ваш код має доступ під час виконання,
> але вони мають різну структуру. Стек зберігає значення в порядку, в якому їх
> отримує, і видаляє їх у зворотньому порядку. Це зветься *останнім надійшов, 
> першим пішов* (англ. "*last in, first out*"). Стек можна уявити, як стос 
> тарілок: коли ви додаєте тарілки, треба ставити їх зверху, а коли треба зняти 
> тарілку, то доводиться брати теж зверху. Додавання чи прибирання тарілок з 
> середини чи знизу стосу матимуть значно гірший наслідок. Додавання данних у
> стек також зветься заштовхуванням, а видалення - відповідно, виштовхуванням.
>
> Стек працює швидко завдяки його способу доступу до даних: йому ніколи не 
> доводиться шукати місце для нових даних чи для звільнення, бо це місце завжди
> на верхівці стеку. Також сприяє швидкій роботі стеку те, що всі дані у ньому
> мають заздалегідь відомий фіксований розмір.
>
> Дані, розмір яких невідомий для нас під час компіляції або розмір яких може
> змінюватися, можна розміщати в купі. Купа менш організована: коли ми 
> розміщуємо дані в купі, ми запитуємо певний обсяг місця. Операційна система
> знаходить достатньо велику пусту ділянку в купі, позначає, що вона 
> використовується, і повертає вказівник на це місце. Цей процес зветься 
> *розміщенням у купі*, що іноді скорочується до простого "розміщення".
> Заштовхування значень у стек не вважається розміщенням. Оскільки вказівник має
> відомий, фіксований розмір, ми можемо зберігати вказівник у стеку, але коли
> нам потрібні власне дані в купі, ми маємо перейти за вказівником.
>
> Уявіть собі столи в ресторані. Коли ви входите до ресторану, вам треба назвати
> кількість людей, що прийшли з вами, тоді офіціант знайде вам пустий стіл, за 
> який всі зможуть сісти, і відведе вас до нього. Якщо хтось спізнився, він 
> зможе спитати, де вас розмістили, щоб приєднатися.
>
> Доступ доданих у купі повільніший, ніж у стеку, бо треба переходити за 
> вказівником, щоб дістатися туди. Сучасні процесори швидше працюють, якщо 
> відбувається менше переходів у пам'яті. Розвинемо аналогію: уявімо офіціанта у
> ресторані, який приймає замовлення з багатьох столів. Найефективніше буде 
> прийняти всі замовлення з одного столу перед тим, як переходити до наступного.
> Приймати замовлення зі столу A, потім зі столу B, потім знову з A і знову з B
> буде значно повільніше. З тієї ж причини процесор краще працює з даними, 
> розташованими поруч (як у стеку), ніж далеко (як може статися в купі). 
> Розміщення великого обсягу даних у купі також може займати багато часу.
>
> Коли ваш код викликає функцію, значення, що передаються у функцію (включно із,
> можливо, вказівниками на дані у купі) і локальні змінні функції заштовхуються
> у стек. Коли функція завершується, ці значення виштовхуються зі стеку.
>
> Відстеження, які частини коду використовують які дані в купі, мінімізація 
> дублювання даних у купі та очищення більше непотрібних даних у купі, щоб не 
> скінчилося місце - ось ті завдання, які покликане розв'язати володіння. Коли
> ви зрозумієте цю концепцію, вам більше не треба буде постійно думати про стек
> і купу, але знання, що причина існування володіння - управління данними у 
> купі, допоможе вам зрозуміти, чому воно працює саме так.
>
<!-- PROD: END BOX -->

### Правила володіння

По-перше, познайомимося із правилами володіння. Тримайте ці правила на увазі, 
поки ми працюватимемо із прикладами, що їх ілюструють:

> 1. Кожне значення в Rust має змінну, що зветься її *власником*.
> 2. У кожен момент може бути лише один власник.
> 3. Коли власник виходить зі зони видимості, значення буде втрачено.

### Область видимості змінної

Ми вже розбирали приклад програми на Rust у Розділі 2. Тепер, оскільки ми вже
знайомі з основами синтаксису, більше не будемо включати всі ці `fn main() {` у
приклади, тому, щоб випробувати їх, вам доведеться помістити ці приклади до
функції `main` самостійно. Завдяки цьому приклади стануть лаконічнішими і 
дозволять зосередитися на важливих деталях, а не на шаблонному коді.

У першому приклад володіння, розглянемо область видимості деяких змінних. 
Область видимості - це фрагмент програми, в якому з елементом можна працювати.
Нехай ми маємо змінну, що виглядає ось так:

```rust
let s = "привіт";
```

Змінна `s` посилається на стрічковий літерал, значення якого жорстко закодовано
в тексті нашої програми. Зі змінною можна працювати з моменту її проголошення до
кінця поточної *області видимості*. Коментарі у Роздруку 4-1 підказують, де 
змінна `s` доступна:

<figure>

```rust
{                      // тут s ще не доступна, її не проголошено
    let s = "hello";   // s доступна з цього місця і надалі

    // щось робимо із s
}                      // область видимості скінчилася, s більше не доступна
```

<figcaption>

Listing 4-1: Змінна і область видимості, де вона доступна

</figcaption>
</figure>

Іншими словами, є два важливих моменти часу:

1. Коли `s` потрапляє у зону видимості, вона стає доступною.
2. Вона лишається такою доки не вийде зі зони видимості.

Поки що, стосунки між областю видимості і доступністю змінних такі ж самі, як і
в інших мовах програмування. З цього почнемо розвиватися, додавши тип `String`.

### Тип `String`

Щоб проілюструвати правила володіння, нам знадобиться тип даних, складніший за 
ті, що ми вже розглянули у Розділі 3. Всі типи даних, які ми розглядали раніше,
зберігаються в стеку і виштовхуються звідти, коли їхня область видимості 
закінчується, але ми хочемо подивитися на дані, що зберігаються в купі і 
подивитися, як Rust дізнається, коли вичищати ці дані.

Скористаємося типом `String` (стрічка) як прикладом і зосередимося на 
особливостях `String`, що стосуються володіння. Ці аспекти також застосовуються
до інших складних типів даних, які надає стандартна бібліотека або ви створюєте
самі. В Розділі 8 ми обговоримо тип `String` детальніше.

Ми вже бачили стрічкові літерали, де значення стрічки жорстко вбито в програму.
Стрічкові літерали зручні, але не завжди підходять для різних ситуацій, де можна
скористатися текстом. Одна з причин полягає в тому, що вони є сталими. Інша - що
не кожне значення стрічки є відомим під час написання коду: наприклад, як взяти
те, що ввів користувач, і зберегти його? Для цих ситуацій, Rust має другий 
стрічковий тип, `String`. Цей тип розміщується в купі і, відтак, може зберігати
текст, обсяг якого невідомий під час компіляції. Можна створити `String` зі 
стрічкового літерали за допомогою функції `from`, ось так:

```rust
let s = String::from("привіт");
```

Подвійна двокрапка (`::`) - це оператор, що дозволяє доступ до простору імен, що
надає нам можливість використати, в цьому випадку, функцію `from` з типу 
`String`, щоб не довелося використоувати назву на кшталт `string_from`. Цей 
синтаксис детальніше обговорюється у секції “Синтакис методів” Розділу 5 і в 
обговоренні простору імен в модулях у Розділі 7.

Цей тип стрічок *може* бути зміненим:

```rust
let mut s = String::from("привіт");

s.push_str(", світе!"); // push_str() дописує літерал до стрічки String

println!("{}", s); // Це виведе `привіт, світе!`
```

У чому ж різниця? Чому `String` може бути зміненим, але літерали - ні? Різниця
полягає в тому, як ці два типи працюють із пам'яттю.

### Пам'ять і розмішення

У випадку стрічкового літералу, ми знаємо вміст під час компіляції, тому текст
жорстко заданий прямо у виконуваному файлі, що робить стрічкові літерали 
швидкими і ефективними. Але ці властивості випливають з його незмінності. На 
жаль, ми не можемо розмістити в двійковому файлі по шмату пам'яті для кожного
фрагменту тексту, розмір яких ми не знаємо під час компіляції і чий розмір може
змінитися під час виконання програми.

З типом `String`, для підтримки несталого шматка тексту, що може зростати, нам
потрібно виділити певну кількість пам'яті в купі, невідому під час компіляції, 
для зберігання вмісту. Це означає:

1. Пам'ять має бути запитана в операційної системи під час виконання.
2. Нам потрібен спосіб повернення цієї пам'яті операційній системі, коли ми 
закінчимо роботу з нашою стрічкою.

Першу частину робимо ми самі: коли ви викликаємо `String::from`, її реалізація
запитує потрібну пам'ять. Це дуже поширено серед мов програмування.

Але друга частина відбувається інакше. У мовах зі *збирачем сміття* (англ. 
garbage collector, GC), саме GC стежить і очищує памя'ть, що більше не 
використовується, і ми, як програмісти, більше можемо не думати про неї. Без GC
на програміста покладається відповідальність за визначення невикористаної 
пам'яті і виклик коду для її повернення, так само, як ми її запитали. Правильно
це робити історично є складною задачею у програмуванні. Якщо ми забудемо, ми
змарнуємо пам'ять. Якщо ми це зробимо зарано, ми матимемо некоректну змінну. 
Якщо ми це зробимо двічі, це теж буде помилкою. Потрібно забезпечити, щоб на 
кожне `виділення` було рівно одне `звільнення` пам'яті.

Rust іде іншим шляхом: пам'ять автоматично повертається, щойно змінна, що нею 
володіла, іде з області видимості. Ось версія нашого прикладу з Роздруку 4-1 із 
використанням `String` замість стрічкового літерала:

```rust
{
    let s = String::from("hello"); // s доступна з цього місця і надалі

    // щось робимо із s
}                                  // область видимості скінчилася, і s тепер
                                   // більше не доступна
```

Існує точка, де природньо можна повернути пам'ять, використану нашою стрічкою,
операційній системі: коли `s` іде з видимості. Коли змінна виходить з видимості,
Rust викликає для нас спеціальну функцію. Ця функція зветься `drop`, і саме там
автор `String` може розмістити код для повернення пам'яті. Rust викликає `drop`
автоматично на закриваючій дужці `}`.
> Примітка: в C++ цей шаблон звільнення ресурсів наприкінці життя об'єкта іноді
> зветься *Отримання ресурсу є ініціалізація* (*Resource Acquisition Is 
> Initialization*, RAII). Функція Rust `drop` знайома вам, якщо ви користувалися
> шаблонами RAII.

Цей шаблон має глибокий вплив на спосіб написання коду Rust. Він наразі може 
виглядати простим, але поведінка коду може бути неочікуваною у складніших 
ситуаціях, коли ми працюватимемо із декількома змінними, що використовують дані,
виділені в купі. Тепер дослідимо деякі з цих ситуацій.

#### Способи взаємодії змінних і даних: переміщення

Числені змінні у Rust можуть взаємодіяти з одними і тими ж даними у різні 
способи. Подивимося на приклад, що використовує ціле число, у Роздруку 4-2:

<figure>

```rust
let x = 5;
let y = x;
```

<figcaption>

Роздрук 4-2: Присвоєння цілого значення змінної `x` змінній `y`

</figcaption>
</figure>

Ми, мабуть, можемо здогадатися, що робить цей код, з нашого досвіду з іншими 
мовами: "прив'язати значення `5` до `x`; потім зробити копію значення з `x` і 
прив'язати її до `y`". Тепер ми маємо дві змінні, `x` та `y`, і обидві 
дорівнюють `5`. І дійсно це так і відбувається, бо цілі - прості значення із 
відомим, фіксованим розміром, і ці два значення `5` внесені у стек.

Тепер подивимося на версію зі `String`:

```rust
let s1 = String::from("hello");
let s2 = s1;
```

Це виглядає дуже схожим на попередній код, тому ми можемо припустити, що воно 
працює так само, тобто другий рядок створить копію значення з `s1` і прив'яже її 
до `s2`. Але тут відбувається щось трохи інше.

Для розлогішого роз'яснення поглянемо на внутрішній устрій 'String' на Рисунку 
4-3. `String` складається з трьох частин, показаних ліворуч: вказівника на 
пам'ять, що збергіає вміст стрічки, довжини, і місткості. Цей набір даних 
зберігається в стеку. Праворуч показана пам'ять у купі, що зберігає вміст.

<figure>
<img alt="String in memory" src="img/trpl04-01.svg" class="center" 
style="width: 50%;" />

<figcaption>

Рисунок 4-3: Представлення в пам'яті стрічки `String` зі значенням `"hello"`, 
прив'язаної до `s1`.

</figcaption>
</figure>

Довжина - це кількість пам'яті, в байтах, що вміст `String` наразі використовує.
The length is how much memory, in bytes, the contents of the `String` is
currently using. The capacity is the total amount of memory, in bytes, that the
`String` has received from the operating system. The difference between length
and capacity matters, but not in this context, so for now, it’s fine to ignore
the capacity.

Коли ми присвоюємо значення `s1` до `s2`, дані `String` копіюються - тобто 
копіюється вказівник, довжина і місткість, що знаходяться в стеку. Ми не 
копіюємо даних у купі, на які посилається вказівник. Іншими словами, 
представлення даних у пам'яті виглядає як на Рисунку 4-4.


<figure>
<img alt="s1 and s2 pointing to the same value" src="img/trpl04-02.svg" 
class="center" style="width: 50%;" />

<figcaption>

Рисунок 4-4: Представлення в пам'яті змінної `s2`, що має копію вказівника, 
довжини і місткості з `s1`.

</figcaption>
</figure>

Представлення *не* виглядає, як показано на Рисунку 4-5, як було б якби Rust 
дійсно копіювала також і дані в купі. Якби Rust так робила, операція `s2 = s1` 
була б потенційно надто витратною з точки зору швидкості виконання, якщо в купі
було б багато даних.

<figure>
<img alt="s1 and s2 to two places" src="img/trpl04-03.svg" class="center" 
style="width: 50%;" />

<figcaption>

Рисуонк 4-5: Інша можливість того, що могло б робити `s2 = s1`, якби Rust 
копіювала також дані в купі.

</figcaption>
</figure>

Раніше ми казали, що коли змінна виходить з області видимості, Rust автоматично
викликає функцію `drop` і очищає пам'ять цієї змінної в купі. Але Рисунок 4-4
показує, що обидва вказівники вказують на одне й те саме місце. Це створює 
проблему: коли `s2` і `s1` вийдуть з видимості, вони удвох спробують звільнити 
одну й ту саму пам'ять. Це зветься помилкою *подвійного звільнення*, і ми про 
неї вже згадували. Звільнення пам'яті двічі може призвести до пошкодження 
пам'яті, і, потенційно, до вразливостей у безпеці.

Для убезпечення пам'яті в цій ситуації в Rust відбувається ще одна дія. Замість
копіювання виділеної пам'яті, Rust вважає `s1` некоректним і, відтак, більше не 
потрібно нічого звільняти, коли `s1` виходить з області видимості. Перевіримо, 
що стається, якщо спробувати використати `s1` після створення `s2`:

```rust,ignore
let s1 = String::from("hello");
let s2 = s1;

println!("{}", s1);
```

Ви отримаєте помилку на кшталт цієї, бо Rust не допускає використання 
некоректних посилань:

```text
error[E0382]: use of moved value: `s1`
 --> src/main.rs:4:27
  |
3 |     let s2 = s1;
  |         -- value moved here
4 |     println!("{}, world!",s1);
  |                           ^^ value used here after move
  |
  = note: move occurs because `s1` has type `std::string::String`,
which does not implement the `Copy` trait
```

Якщо ви чули терміни "пласка копія" та "глибока копія" (“shallow copy” та “deep 
copy” відповідно), коли працювали з іншими мовами, поняття копіювання 
вказівника, довжини і місткості без копіювання даних виглядають для вас схожими
на пласку копію. Але оскільки Rust також унепридатнює першу змінну, це зветься
не пласкою копією, а *переміщенням*. Тут надалі вживатиметься вираз `s1` було 
*переміщено* в `s2`. Що відбувається в дійсності, показано на Рисунку 4-6.

<figure>
<img alt="s1 moved to s2" src="img/trpl04-04.svg" class="center" 
style="width: 50%;" />

<figcaption>

Рисунок 4-6: Представлення в пам'яті після унепридатнення `s1`

</figcaption>
</figure>

Це вирішує нашу проблему! Якщо коректним зосталося лише `s2`, коли воно вийде з
видимості, то саме звільнить пам'ять, і готово.

На додачу, такий дизайн мови неявно гарантує, що Rust ніколи не буде автоматично
створювати "глибокі" копії ваших даних. Таким чином, будь-яке *автоматичне* 
копіювання може вважатися недорогим з точки зору продуктивності під час 
виконання.

#### Ways Variables and Data Interact: Clone

If we *do* want to deeply copy the heap data of the `String`, not just the
stack data, we can use a common method called `clone`. We’ll discuss method
syntax in Chapter 5, but because methods are a common feature in many
programming languages, you’ve probably seen them before.

Here’s an example of the `clone` method in action:

```rust
let s1 = String::from("hello");
let s2 = s1.clone();

println!("s1 = {}, s2 = {}", s1, s2);
```

This works just fine and is how you can explicitly produce the behavior shown
in Figure 4-5, where the heap data *does* get copied.

When you see a call to `clone`, you know that some arbitrary code is being
executed and that code may be expensive. It’s a visual indicator that something
different is going on.

#### Stack-Only Data: Copy

There’s another wrinkle we haven’t talked about yet. This code using integers,
part of which was shown earlier in Listing 4-2, works and is valid:

```rust
let x = 5;
let y = x;

println!("x = {}, y = {}", x, y);
```

But this code seems to contradict what we just learned: we don’t have a call to
`clone`, but `x` is still valid and wasn’t moved into `y`.

The reason is that types like integers that have a known size at compile time
are stored entirely on the stack, so copies of the actual values are quick to
make. That means there’s no reason we would want to prevent `x` from being
valid after we create the variable `y`. In other words, there’s no difference
between deep and shallow copying here, so calling `clone` wouldn’t do anything
differently from the usual shallow copying and we can leave it out.

Rust has a special annotation called the `Copy` trait that we can place on
types like integers that are stored on the stack (we’ll talk more about traits
in Chapter 10). If a type has the `Copy` trait, an older variable is still
usable after assignment. Rust won’t let us annotate a type with the `Copy`
trait if the type, or any of its parts, has implemented the `Drop` trait. If
the type needs something special to happen when the value goes out of scope and
we add the `Copy` annotation to that type, we’ll get a compile time error.

So what types are `Copy`? You can check the documentation for the given type to
be sure, but as a general rule, any group of simple scalar values can be
`Copy`, and nothing that requires allocation or is some form of resource is
`Copy`. Here are some of the types that are `Copy`:

* All the integer types, like `u32`.
* The boolean type, `bool`, with values `true` and `false`.
* All the floating point types, like `f64`.
* Tuples, but only if they contain types that are also `Copy`. `(i32, i32)` is
`Copy`, but `(i32, String)` is not.

### Ownership and Functions

The semantics for passing a value to a function are similar to assigning a
value to a variable. Passing a variable to a function will move or copy, just
like assignment. Listing 4-7 has an example with some annotations showing where
variables go into and out of scope:

<figure>
<span class="filename">Filename: src/main.rs</span>

```rust
fn main() {
    let s = String::from("hello");  // s comes into scope.

    takes_ownership(s);             // s's value moves into the function...
                                    // ... and so is no longer valid here.
    let x = 5;                      // x comes into scope.

    makes_copy(x);                  // x would move into the function,
                                    // but i32 is Copy, so it’s okay to still
                                    // use x afterward.

} // Here, x goes out of scope, then s. But since s's value was moved, nothing
  // special happens.

fn takes_ownership(some_string: String) { // some_string comes into scope.
    println!("{}", some_string);
} // Here, some_string goes out of scope and `drop` is called. The backing
  // memory is freed.

fn makes_copy(some_integer: i32) { // some_integer comes into scope.
    println!("{}", some_integer);
} // Here, some_integer goes out of scope. Nothing special happens.
```

<figcaption>

Listing 4-7: Functions with ownership and scope annotated

</figcaption>
</figure>

If we tried to use `s` after the call to `takes_ownership`, Rust would throw a
compile time error. These static checks protect us from mistakes. Try adding
code to `main` that uses `s` and `x` to see where you can use them and where
the ownership rules prevent you from doing so.

### Return Values and Scope

Returning values can also transfer ownership. Here’s an example with similar
annotations to those in Listing 4-7:

<span class="filename">Filename: src/main.rs</span>

```rust
fn main() {
    let s1 = gives_ownership();         // gives_ownership moves its return
                                        // value into s1.

    let s2 = String::from("hello");     // s2 comes into scope.

    let s3 = takes_and_gives_back(s2);  // s2 is moved into
                                        // takes_and_gives_back, which also
                                        // moves its return value into s3.
} // Here, s3 goes out of scope and is dropped. s2 goes out of scope but was
  // moved, so nothing happens. s1 goes out of scope and is dropped.

fn gives_ownership() -> String {             // gives_ownership will move its
                                             // return value into the function
                                             // that calls it.

    let some_string = String::from("hello"); // some_string comes into scope.

    some_string                              // some_string is returned and
                                             // moves out to the calling
                                             // function.
}

// takes_and_gives_back will take a String and return one.
fn takes_and_gives_back(a_string: String) -> String { // a_string comes into
                                                      // scope.

    a_string  // a_string is returned and moves out to the calling function.
}
```

The ownership of variables follows the same pattern every time: assigning a
value to another variable moves it, and when heap data values’ variables go out
of scope, if the data hasn’t been moved to be owned by another variable, the
value will be cleaned up by `drop`.

Taking ownership and then returning ownership with every function is a bit
tedious. What if we want to let a function use a value but not take ownership?
It’s quite annoying that anything we pass in also needs to be passed back if we
want to use it again, in addition to any data resulting from the body of the
function that we might want to return as well.

It’s possible to return multiple values using a tuple, like this:

<span class="filename">Filename: src/main.rs</span>

```rust
fn main() {
    let s1 = String::from("hello");

    let (s2, len) = calculate_length(s1);

    println!("The length of '{}' is {}.", s2, len);
}

fn calculate_length(s: String) -> (String, usize) {
    let length = s.len(); // len() returns the length of a String.

    (s, length)
}
```

But this is too much ceremony and a lot of work for a concept that should be
common. Luckily for us, Rust has a feature for this concept, and it’s called
*references*.
