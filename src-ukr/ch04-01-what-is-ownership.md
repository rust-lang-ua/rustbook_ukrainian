## Що таке володіння?

*Ownership* is a set of rules that govern how a Rust program manages memory. Усі програми мають керувати тим, як вони використовують пам'ять комп'ютера під час роботи. Some languages have garbage collection that regularly looks for no-longer-used memory as the program runs; in other languages, the programmer must explicitly allocate and free the memory. Rust використовує третій підхід: пам'ять управляється системою володіння з набором правил, які перевіряє компілятор. Якщо якесь із правил порушується, програма не скомпілюється. Жодна з особливостей володіння не сповільніть вашу програму під час виконання.

Оскільки володіння - нова концепція для багатьох програмістів, потрібен деякий час, щоб звикнути до нього. Добра новина - що досвідченішим ви ставатимете в Rust і правилах системи володіння, тим легшим для вас буде природно писати безпечний і ефективний код. Не здавайтеся!

Коли ви зрозумієте володіння, ви матимете надійну основу для розуміння особливостей, що роблять Rust унікальною мовою. В цьому розділі ви вивчите володіння, працюючи з прикладами, що зосереджуються на добре відомих структурах даних: стрічках.

> ### Стек і купа
> 
> У багатьох мовах програмування програміст нечасто має думати про стек і купу. Але в системних мовах, таких, як Rust, розташування значення в стеку чи в купі впливає на поведінку програми й змушує вас ухвалювати певні рішення. Деталі володіння будуть описані стосовно стека та купи пізніше у цьому розділі, а тут коротке пояснення для підготовки.
> 
> І стек, і купа - частини пам'яті, до яких ваш код має доступ під час виконання, але вони мають різну структуру. Стек зберігає значення в порядку, в якому їх отримує, і видаляє їх у зворотньому порядку. Це зветься *останнім надійшов, першим пішов* (<0>last in, first out</0>). Стек можна уявити, як стос тарілок: коли ви додаєте тарілки, треба ставити їх зверху, а коли треба зняти тарілку, то доводиться брати теж зверху. Додавання чи прибирання тарілок з середини чи знизу стосу матимуть значно гірший наслідок! Додавання даних також зветься *заштовхуванням у стек* (*push*), а видалення - відповідно, <0>виштовхуванням</0> (<0>pop</0>). Усі дані. що зберігаються в стеку, мають бути відомого і незмінного розміру. Дані, розмір яких невідомий під час компіляції, або може змінитися, мають зберігатися в купі.
> 
> Купа менш організована: коли ви розміщуєте дані в купі, то запитуєте певний обсяг місця. Програма-розподілювач знаходить достатньо велику порожню ділянку в купі, позначає, що вона використовується, і повертає *вказівник*, тобто адресу цього місця. Цей процес зветься *розподілом у купі*, що іноді скорочується до простого *розподілом* (заштовхування значень до стека не вважається розподілом). Оскільки вказівник на купу має відомий, постійний розмір, ви можете зберегти цей вказівник у стеку, але коли вам потрібні дані, вам треба перейти за вказівником. Уявіть собі столи в ресторані. When you enter, you state the number of people in your group, and the host finds an empty table that fits everyone and leads you there. Якщо хтось спізнився, він зможе спитати, де вас розмістили, щоб приєднатися.
> 
> Заштовхування до стека швидше за розподіл у купі, оскільки розподілювачу не треба шукати місце для нових даних, бо це місце завжди є вершиною стека. Comparatively, allocating space on the heap requires more work because the allocator must first find a big enough space to hold the data and then perform bookkeeping to prepare for the next allocation.
> 
> Доступ доданих у купі повільніший, ніж у стеку, бо треба переходити за вказівником, щоб дістатися туди. Сучасні процесори швидше працюють, якщо відбувається менше переходів у пам'яті. Розвинемо аналогію: уявімо офіціанта у ресторані, який приймає замовлення з багатьох столів. Найефективніше буде > прийняти всі замовлення з одного столу перед тим, як переходити до наступного. Приймати замовлення зі столу A, потім зі столу B, потім знову з A і знову з B буде значно повільніше. З тієї ж причини процесор краще працює з даними, розташованими поруч (як у стеку), ніж далеко (як може статися в купі).
> 
> Коли ваш код викликає функцію, значення, що передаються у функцію (включно з, можливо, вказівниками на дані у купі) і локальні змінні функції заштовхуються у стек. Коли функція завершується, ці значення виштовхуються зі стека.
> 
> Відстеження, які частини коду використовують які дані в купі, мінімізація дублювання даних у купі та очищення даних у купі, що вже не потрібні, щоб не скінчилося місце - ось ті завдання, які покликане розв'язати володіння. Коли ви зрозумієте концепцію володіння, вам більше не треба буде постійно думати про стек і купу, але знання, що причина існування володіння - управління даними у купі, допоможе вам зрозуміти, чому воно працює саме так.

### Правила володіння

По-перше, познайомимося із правилами володіння. Тримайте ці правила на увазі, поки ми працюватимемо із прикладами, що їх ілюструють:

* Кожне значення в Rust має *власника*.
* У кожен момент може бути лише один власник.
* Коли власник виходить зі зони видимості, значення буде скинуто.

### Область видимості змінної

Тепер, оскільки ми вже знайомі з основами синтаксису Rust, більше не будемо включати всі ці `fn main() {` у приклади, тому, щоб випробувати їх, вам доведеться помістити ці приклади до функції `main` самостійно. Завдяки цьому приклади стануть лаконічнішими і дозволять зосередитися на важливих деталях, а не на шаблонному коді.

У першому прикладі володіння ми розглянемо *область видимості* деяких змінних. Область видимості - це фрагмент програми, в якому з елементом можна працювати. Нехай ми маємо ось таку змінну:

```rust
let s = "hello";
```

Змінна `s` посилається на стрічковий літерал, значення якого жорстко задане в тексті нашої програми. Зі змінною можна працювати з моменту її проголошення до кінця поточної *області видимості*. Коментарі у Блоці коду 4-1 підказують, де змінна `s` доступна.

```rust
{{#rustdoc_include ../listings/ch04-understanding-ownership/listing-04-01/src/main.rs:here}}
```


<span class="caption">Listing 4-1: A variable and the scope in which it is valid</span>

Іншими словами, є два важливі моменти часу:

* When `s` comes *into* scope, it is valid.
* It remains valid until it goes *out of* scope.

Поки що стосунки між областями видимості та доступністю змінних такі ж самі, як і в інших мовах програмування. Тепер, спираючися на це розуміння, будемо розвиватися, додавши тип `String`.

### Тип `String`

Щоб проілюструвати правила володіння, нам знадобиться тип даних, складніший за ті, що ми вже розглянули у підрозділі [“Типи даних”][data-types]<!-- ignore --> Розділу 3. The types covered previously are of a known size, can be stored on the stack and popped off the stack when their scope is over, and can be quickly and trivially copied to make a new, independent instance if another part of code needs to use the same value in a different scope. Але тепер ми розглянемо дані, що зберігаються в купі та подивимося, як Rust дізнається, коли ці дані треба вичищати, і тип `String` є чудовим прикладом.

Ми зосередимося на особливостях `String`, що стосуються володіння. Ці аспекти також застосовуються до інших складних типів даних, які надає стандартна бібліотека або ви створюєте самі. Ми поговоримо про `String` більш детально в [Розділі 8][ch8]<!-- ignore -->.

Ми вже бачили стрічкові літерали, де значення стрічки жорстко задане в програмі. Стрічкові літерали зручні, але не завжди підходять для різних ситуацій, де виникає потреба скористатися текстом. Одна з причин полягає в тому, що вони є сталими. Інша - що не кожне значення стрічки є відомим під час написання коду: наприклад, як взяти те, що ввів користувач, і зберегти його? Для цих ситуацій, Rust має другий стрічковий тип, `String`. Цей тип керує даними, розподіленими в купі й, відтак, може зберігати текст, обсяг якого невідомий під час компіляції. Можна створити `String` зі стрічкового літерала за допомогою функції `from`, ось так:

```rust
let s = String::from("hello");
```

Оператор подвійна двокрапка `::` дозволяє доступ до простору імен, що надає нам можливість використати, в цьому випадку, функцію `from` з типу `String`, щоб не довелося використовувати назву на кшталт `string_from`. Цей синтаксис детальніше обговорюється у підрозділі [“Синтакис методів”][method-syntax]<!-- ignore --> section of Chapter 5, and when we talk about namespacing with modules in [“Paths for Referring to an Item in the Module Tree”][paths-module-tree]<!-- ignore --> Розділу 7.

Цей тип стрічок *може* бути зміненим:

```rust
{{#rustdoc_include ../listings/ch04-understanding-ownership/no-listing-01-can-mutate-string/src/main.rs:here}}
```

У чому ж різниця? Чому `String` може бути зміненим, але літерали - ні? The difference is in how these two types deal with memory.

### Пам'ять і розподіл

У випадку стрічкового літерала ми знаємо його вміст під час компіляції, тому текст жорстко заданий прямо у виконуваному файлі, що робить стрічкові літерали швидкими і ефективними. Але ці властивості випливають з незмінності літерала. На жаль, ми не можемо розмістити у двійковому файлі по шмату пам'яті для кожного фрагменту тексту, розмір якого ми не знаємо під час компіляції й чий розмір може змінитися під час виконання програми.

Для типу `String`, задля підтримки несталого шматка тексту, що може зростати, нам потрібно розподілити певну кількість пам'яті в купі, невідому під час компіляції, для зберігання вмісту. Це означає:

* Пам'ять має бути запитана в розподілювача під час виконання.
* We need a way of returning this memory to the allocator when we’re done with our `String`.

Першу частину робимо ми самі: коли ми викликаємо `String::from`, її реалізація запитує потрібну пам'ять. Так роблять практично всі мови програмування.

Але друга частина відбувається інакше. У мовах зі *збирачем сміття (garbage collector, GC)*, саме GC стежить і очищує пам'ять, що більше не використовується, і ми, як програмісти, більше можемо не думати про неї. In most languages without a GC, it’s our responsibility to identify when memory is no longer being used and to call code to explicitly free it, just as we did to request it. Правильно це робити історично є складною задачею у програмуванні. Якщо ми забудемо, ми змарнуємо пам'ять. Якщо ми це зробимо зарано, ми матимемо некоректну змінну. Якщо ми це зробимо двічі, це теж буде помилкою. Потрібно забезпечити, щоб на кожен `розподіл` було рівно одне `звільнення` пам'яті.

Rust іде іншим шляхом: пам'ять автоматично повертається, щойно змінна, що нею володіла, іде з області видимості. Ось версія нашого прикладу з Блоку коду 4-1 із використанням `String` замість стрічкового літерала:

```rust
{{#rustdoc_include ../listings/ch04-understanding-ownership/no-listing-02-string-scope/src/main.rs:here}}
```

Існує точка, де природно можна повернути пам'ять, використану нашою стрічкою, розподілювачу: коли `s` іде з області видимості. Коли змінна виходить з області видимості, Rust викликає для нас спеціальну функцію. Ця функція зветься [`drop`][drop]<!-- ignore -->, і саме там автор `String` може розмістити код для повернення пам'яті. Rust викликає `drop` автоматично на закриваючій фігурній дужці.

> Примітка: в C++ цей шаблон звільнення ресурсів наприкінці життя об'єкта іноді зветься *Отримання ресурсу є ініціалізація* (<0>Resource Acquisition Is Initialization, RAII</0>). Функція Rust `drop` має бути знайома вам, якщо ви користувалися шаблонами RAII.

Цей шаблон має глибокий вплив на спосіб написання коду Rust. Він наразі може виглядати простим, але поведінка коду може бути неочікуваною у складніших ситуаціях, коли ми працюватимемо із декількома змінними, що використовують дані, розподіленими в купі. Тепер дослідимо деякі з цих ситуацій.

<!-- Old heading. Do not remove or links may break. -->
<a id="ways-variables-and-data-interact-move"></a>

#### Variables and Data Interacting with Move

Multiple variables can interact with the same data in different ways in Rust. Let’s look at an example using an integer in Listing 4-2.

```rust
{{#rustdoc_include ../listings/ch04-understanding-ownership/listing-04-02/src/main.rs:here}}
```


<span class="caption">Listing 4-2: Assigning the integer value of variable `x` to `y`</span>

We can probably guess what this is doing: “bind the value `5` to `x`; then make a copy of the value in `x` and bind it to `y`.” We now have two variables, `x` and `y`, and both equal `5`. This is indeed what is happening, because integers are simple values with a known, fixed size, and these two `5` values are pushed onto the stack.

Now let’s look at the `String` version:

```rust
{{#rustdoc_include ../listings/ch04-understanding-ownership/no-listing-03-string-move/src/main.rs:here}}
```

This looks very similar, so we might assume that the way it works would be the same: that is, the second line would make a copy of the value in `s1` and bind it to `s2`. But this isn’t quite what happens.

Take a look at Figure 4-1 to see what is happening to `String` under the covers. A `String` is made up of three parts, shown on the left: a pointer to the memory that holds the contents of the string, a length, and a capacity. This group of data is stored on the stack. On the right is the memory on the heap that holds the contents.

<img alt="Two tables: the first table contains the representation of s1 on the
stack, consisting of its length (5), capacity (5), and a pointer to the first
value in the second table. The second table contains the representation of the
string data on the heap, byte by byte." src="img/trpl04-01.svg" class="center"
style="width: 50%;" />

<span class="caption">Figure 4-1: Representation in memory of a `String` holding the value `"hello"` bound to `s1`</span>

The length is how much memory, in bytes, the contents of the `String` are currently using. Місткість - це загальний обсяг пам'яті в байтах, що `String` отримала від розподілювача. The difference between length and capacity matters, but not in this context, so for now, it’s fine to ignore the capacity.

Коли ми присвоюємо значення `s1` змінній `s2`, дані `String` копіюються - тобто копіюється вказівник, довжина і місткість, що знаходяться в стеку. Ми не копіюємо даних у купі, на які посилається вказівник. Іншими словами, представлення даних у пам'яті виглядає як на Рисунку 4-2.

<img alt="Three tables: tables s1 and s2 representing those strings on the
stack, respectively, and both pointing to the same string data on the heap."
src="img/trpl04-02.svg" class="center" style="width: 50%;" />

<span class="caption">Figure 4-2: Representation in memory of the variable `s2` that has a copy of the pointer, length, and capacity of `s1`</span>

Представлення *не* виглядає, як показано на Рисунку 4-3, як було б якби Rust дійсно копіювала також і дані в купі. Якби Rust так робила, операція `s2 = s1` була б потенційно дуже витратною з точки зору швидкості виконання, якщо в купі було б багато даних.

<img alt="Four tables: two tables representing the stack data for s1 and s2,
and each points to its own copy of string data on the heap."
src="img/trpl04-03.svg" class="center" style="width: 50%;" />

<span class="caption">Figure 4-3: Another possibility for what `s2 = s1` might do if Rust copied the heap data as well</span>

Раніше ми казали, що коли змінна виходить з області видимості, Rust автоматично викликає функцію `drop` і очищає пам'ять цієї змінної в купі. Але Рисунок 4-2 показує, що обидва вказівники вказують на одне й те саме місце. Це створює проблему: коли `s2` і `s1` вийдуть з області видимості, вони удвох спробують звільнити одну й ту саму пам'ять. Це зветься помилкою *подвійного звільнення*, і ми про неї вже згадували. Звільнення пам'яті двічі може призвести до пошкодження пам'яті, і, потенційно, до вразливостей у безпеці.

To ensure memory safety, after the line `let s2 = s1;`, Rust considers `s1` as no longer valid. Відтак, Rust тепер не буде нічого звільняти, коли `s1` вийде з області видимості. Перевірте, що станеться, коли ви спробуєте використати `s1` після створення `s2`; це не спрацює:

```rust,ignore,does_not_compile
{{#rustdoc_include ../listings/ch04-understanding-ownership/no-listing-04-cant-use-after-move/src/main.rs:here}}
```

You’ll get an error like this because Rust prevents you from using the invalidated reference:

```console
{{#include ../listings/ch04-understanding-ownership/no-listing-04-cant-use-after-move/output.txt}}
```

Якщо ви чули терміни *пласка копія* та *глибока копія* (“shallow copy” та “deep copy”), коли працювали з іншими мовами, поняття копіювання вказівника, довжини та місткості без копіювання даних виглядають для вас схожими на пласку копію. But because Rust also invalidates the first variable, instead of being called a shallow copy, it’s known as a *move*. In this example, we would say that `s1` was *moved* into `s2`. So, what actually happens is shown in Figure 4-4.

<img alt="Three tables: tables s1 and s2 representing those strings on the
stack, respectively, and both pointing to the same string data on the heap.
Table s1 is grayed out be-cause s1 is no longer valid; only s2 can be used to
access the heap data." src="img/trpl04-04.svg" class="center" style="width:
50%;" />

<span class="caption">Figure 4-4: Representation in memory after `s1` has been invalidated</span>

Це вирішує нашу проблему! With only `s2` valid, when it goes out of scope it alone will free the memory, and we’re done.

На додачу, такий дизайн мови неявно гарантує, що Rust ніколи не буде автоматично створювати "глибокі" копії ваших даних. Таким чином, будь-яке *автоматичне* копіювання може вважатися недорогим з точки зору продуктивності під час виконання.

<!-- Old heading. Do not remove or links may break. -->
<a id="ways-variables-and-data-interact-clone"></a>

#### Variables and Data Interacting with Clone

Якщо ми *хочемо* зробити глибоку копію даних `String` у купі, а не лише в стеку, ми можемо використати загальний метод, що зветься `clone`. Синтаксис використання методів буде обговорено в Розділі 5, але оскільки методи є загальною особливістю багатьох мов програмування, ви, швидше за все, вже бачили їх.

Ось приклад застосування методу `clone`:

```rust
{{#rustdoc_include ../listings/ch04-understanding-ownership/no-listing-05-clone/src/main.rs:here}}
```

This works just fine and explicitly produces the behavior shown in Figure 4-3, where the heap data *does* get copied.

Коли ви бачите виклик `clone`, ви знаєте, що виконується певний визначений код і цей код може коштувати продуктивності. Це візуальний індикатор, що відбувається певна операція.

#### Дані в стеку: копіювання

Є ще одна дрібниця, про яку ми ще не говорили. This code using integers—part of which was shown in Listing 4-2—works and is valid:

```rust
{{#rustdoc_include ../listings/ch04-understanding-ownership/no-listing-06-copy/src/main.rs:here}}
```

But this code seems to contradict what we just learned: we don’t have a call to `clone`, but `x` is still valid and wasn’t moved into `y`.

Причина у тому, що типи на кшталт цілих, що мають відомий розмір часу компіляції, зберігаються повністю в стеку, тому копіювання їхніх значень відбувається швидко. Це означає, що нема підстав запобігати коректності `x` після створення змінної `y`. In other words, there’s no difference between deep and shallow copying here, so calling `clone` wouldn’t do anything different from the usual shallow copying, and we can leave it out.

Rust має спеціальне позначення, що зветься трейтом `Copy`, який можна додати до типів на кшталт цілих, що зберігаються в стеку (детальніше трейти обговорюються в [Розділі 10][traits]<!-- ignore -->). Якщо тип реалізовує трейт `Copy`, змінні, що використовують його не переміщуються, а банально копіюються, що робить їх коректними після присвоєння іншій змінній.

Rust не дозволить позначити тип трейтом `Copy`, якщо тип, чи якась з його частин, реалізовує``трейт`Drop``. Якщо тип потребує чогось особливого, коли змінна виходить з області видимості, і ми додаємо позначення `Copy` до цього типу, ми отримаємо помилку часу компіляції. Щоб дізнатися про те, як додати позначку `Copy` до вашого типу для реалізації трейта, див. ["Придатні до успадкування трейти"][derivable-traits]<!-- ignore --> у Додатку C.

So, what types implement the `Copy` trait? Можна перевірити документацію до певного типу, щоб бути певним, але загальне правило таке: будь-яка група простих скалярних значень може реалізовувати `Copy`, і нічого з того, що потребує розподілу пам'яті чи є ресурсом, не є `Copy`. Ось кілька типів, що реалізовують `Copy`:

* Всі цілі типи, на кшталт `u32`.
* Булевий тип, `bool`, значення якого `true` та `false`.
* All the floating-point types, such as `f64`.
* Символьний тип, `char`.
* Кортежі, якщо вони містять лише типи, що реалізовують `Copy`. Скажімо, `(i32, i32)` реалізовує `Copy`, але `(i32, String)` - ні.

### Володіння та функції

Механіка передачі значень функції подібна до присвоювання значення змінній. Передача змінної функції є переміщенням чи копією, як і присвоювання. Блок коду 4-7 містить приклад з певними поясненнями, що розкривають, де змінні входять і виходять з області видимості.

<span class="filename">Файл: src/main.rs</span>

```rust
{{#rustdoc_include ../listings/ch04-understanding-ownership/listing-04-03/src/main.rs}}
```


<span class="caption">Listing 4-3: Functions with ownership and scope annotated</span>

Якби ми спробували використати `s` після виклику `takes_ownership`, Rust повідомив би про помилку часу компіляції. Ці статичні перевірки захищають нас від помилок. Спробуйте додати в `main` код, що використовує `s` та `x`, щоб побачити, де їх можна використовувати, а де правила володіння запобігають цьому.

### Повернення значень та область видимості

Повернення значень також передає володіння. Listing 4-4 shows an example of a function that returns some value, with similar annotations as those in Listing 4-3.

<span class="filename">Файл: src/main.rs</span>

```rust
{{#rustdoc_include ../listings/ch04-understanding-ownership/listing-04-04/src/main.rs}}
```


<span class="caption">Listing 4-4: Transferring ownership of return values</span>

Володіння змінними завше дотримується однакової схеми: присвоєння значення іншій змінній переміщує його. Коли змінна, що включає дані в купі, виходять з області видимості, якщо дані не були переміщені у володіння іншої змінної, значення буде очищене викликом `drop`.

Хоча так код працює, все ж взяття володіння і повернення володіння в кожній функції дещо втомлює. Що, як ми хочемо дозволити функції використати значення, але не брати володіння? Потреба повертати все, що ми передаємо в функції, щоб його можна було знову використовувати, разом із даними, утвореними в результаті роботи функції, дратує.

Можна повертати багато значень кортежем, як показано в Блоці коду 4-5.

<span class="filename">Файл: src/main.rs</span>

```rust
{{#rustdoc_include ../listings/ch04-understanding-ownership/listing-04-05/src/main.rs}}
```

<span class="caption">Блок коду 4-5: Повернення володіння параметрами</span>

Але це б давало забагато ритуальних рухів і зайвої роботи для концепції, що має бути загальновживаною. На щастя для нас, Rust має засіб для використання змінної без передачі володіння, що зветься *посиланнями*.

[data-types]: ch03-02-data-types.html#data-types
[ch8]: ch08-02-strings.html
[traits]: ch10-02-traits.html
[derivable-traits]: appendix-03-derivable-traits.html
[method-syntax]: ch05-03-method-syntax.html#method-syntax
[paths-module-tree]: ch07-03-paths-for-referring-to-an-item-in-the-module-tree.html
[drop]: ../std/ops/trait.Drop.html#tymethod.drop
