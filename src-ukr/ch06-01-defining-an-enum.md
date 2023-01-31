## Визначення enum-а

Якщо структури надають спосіб групування пов'язаних полів і даних, як `Rectangle` з його `width` і `height`, то енуми дають вам спосіб виразити значення, що є одним з можливого набору значень. Скажімо, ми хочемо сказати, що `Rectangle` є однією з можливих фігур, які також включають `Circle`(круг) і `Triangle`(трикутник). Для цього Rust надає нам можливість закодувати ці варіанти у енум.

Розгляньмо ситуацію, яку ми можемо захотіти виразити в коді, і побачимо, чому енуми корисні і краще за структури підходять для цієї ситуації. Нехай нам потрібно працювати із IP-адресами. Наразі використовується два стандарти IP-адрес, четверта та шоста версії. Оскільки це єдині можливі IP-адреси, які наша програма може зустріти, ми можемо *перелічити* (enumerate) усі можливі варіанти, звідси й назва для енумів.

Будь-яка IP-адреса може бути або версії чотири, або версії шість, але не одночасно. That property of IP addresses makes the enum data structure appropriate because an enum value can only be one of its variants. Адреси як четвертої, так і шостої версій засадничо є саме IP-адресами, і з ними можна працювати як з одним типом, коли код стосується ситуацій, де можуть використовуватися обидва види адрес.

Цю концепцію можна виразити, визначивши енум `IpAddrKind` і перерахувавши можливі види IP-адрес, `V4` та `V6`. Це зветься варіантами енума:

```rust
{{#rustdoc_include ../listings/ch06-enums-and-pattern-matching/no-listing-01-defining-enums/src/main.rs:def}}
```

`IpAddrKind` тепер є користувацьким типом даних, яким ми можемо користуватися деінде в нашому коді.

### Значення енума

Ми можемо створити екземпляри обох варіантів `IpAddrKind` таким чином:

```rust
{{#rustdoc_include ../listings/ch06-enums-and-pattern-matching/no-listing-01-defining-enums/src/main.rs:instance}}
```

Зверніть увагу, що варіанти енума знаходяться у просторі імен його ідентифікатора, і для з'єднання ми використовуємо подвійну двокрапку. Це корисно, бо значення `IpAddrKind::V4` і `IpAddrKind::V6` належать до одного типу `IpAddrKind`. Тепер можна, скажімо, визначити функцію, що приймає `IpAddrKind`:

```rust
{{#rustdoc_include ../listings/ch06-enums-and-pattern-matching/no-listing-01-defining-enums/src/main.rs:fn}}
```

І ми можемо викликати цю функцію для будь-якого з варіантів:

```rust
{{#rustdoc_include ../listings/ch06-enums-and-pattern-matching/no-listing-01-defining-enums/src/main.rs:fn_call}}
```

Але використання enum-ів дає ще більше переваг. Наразі ми не маємо способу зберігати власне *дані* IP-адреси; ми знаємо лише її *вид*. Оскільки ви щойно дізналися про структури в Розділі 5, у вас може виникнути спокуса розв'язати цю проблему структурами, як показано у Блоці коду 6-1.

```rust
{{#rustdoc_include ../listings/ch06-enums-and-pattern-matching/listing-06-01/src/main.rs:here}}
```


<span class="caption">Listing 6-1: Storing the data and `IpAddrKind` variant of an IP address using a `struct`</span>

Тут ми визначили структуру `IpAddr`, що має два поля: `kind` (вид) типу `IpAddrKind` (щойно визначений нами енум) та `address` типу `String`. Ми маємо два екземпляри цієї структури. Перший, `home`, має значення `IpAddrKind::V4` в полі `kind` і прив'язані дані адреси `127.0.0.1`. Другий екземпляр, `loopback`, має значенням поля `kind` інший варіант `IpAddrKind` - `V6`, і має прив'язану адресу `::1`. Ми використали структуру, щоб пов'язати значення `kind` та `address` разом, таким чином варіант тепер прив'язаний до значення.

Але цю концепцію можна представити у коротший спосіб за допомогою самого енума, а не енума всередині структури, розмістивши дані безпосередньо в кожному варіанті енума. Це нове визначення енума `IpAddr` каже, що обидва варіанти `V4` та `V6` мають прив'язані значення `String`:

```rust
{{#rustdoc_include ../listings/ch06-enums-and-pattern-matching/no-listing-02-enum-with-data/src/main.rs:here}}
```

Ми причепили дані безпосередньо до кожного варіанту енума, і тепер нема потреби в додатковій структурі. Here, it’s also easier to see another detail of how enums work: the name of each enum variant that we define also becomes a function that constructs an instance of the enum. Тобто `IpAddr::V4()` - це виклик функції, що приймає аргументом `String` і повертає екземпляр типу `IpAddr`. Ми автоматично отримуємо функцію-конструктор, визначену в результаті визначення енума.

Є ще одна перевага у використанні енума перед структурою: кожен варіант може мати різні типи та об'єм прив'язаних даних. Version four IP addresses will always have four numeric components that will have values between 0 and 255. Якби ми хотіли зберігати адреси `V4` як чотири значення `u8`, але все ще представляти `V6` як єдине значення типу `String`, то структурою ми б цього зробити не змогли. Натомість енуми легко впораються із цим:

```rust
{{#rustdoc_include ../listings/ch06-enums-and-pattern-matching/no-listing-03-variants-with-different-data/src/main.rs:here}}
```

Ми представили кілька різних способів визначення структури даних для зберігання IP-адрес версій чотири та шість. Однак, як виявляється, бажання зберігати IP-адреси і кодувати їхній вид настільки поширене, що [стандартна бібліотека вже містить визначення, яке можна використати!][IpAddr]<!-- ignore --> Подивімося, як стандартна бібліотека визначає `IpAddr`: там є точно такий enum і варіанти, як і ті, що ми визначили, але дані адрес усередині варіантів представлені двома різними структурами, які визначені окремо для кожного варіанту:

```rust
struct Ipv4Addr {
    // --snip--
}

struct Ipv6Addr {
    // --snip--
}

enum IpAddr {
    V4(Ipv4Addr),
    V6(Ipv6Addr),
}
```

Цей код показує, що ми можемо помістити будь-який вид даних усередину варіанту енума: стрічки, числові типи, структури тощо. Можна навіть вкласти інший енум! Також типи стандартної бібліотеки часто не набагато складніші за те, що ви б могли самі придумати.

Зверніть увагу, що хоча стандартна бібліотека містить визначення `IpAddr`, ми можемо створити і користуватися нашим власним визначенням без конфлікту, бо ми не ввели визначення зі стандартної бібліотеки до області видимості програми. Ми ще поговоримо про введення до області видимості в Розділі 7.

Let’s look at another example of an enum in Listing 6-2: this one has a wide variety of types embedded in its variants.

```rust
{{#rustdoc_include ../listings/ch06-enums-and-pattern-matching/listing-06-02/src/main.rs:here}}
```


<span class="caption">Listing 6-2: A `Message` enum whose variants each store different amounts and types of values</span>

Цей енум має чотири варіанти різних типів:

* `Quit` ("вийти") не має пов'язаних даних.
* `Move` has named fields, like a struct does.
* `Write` ("написати") включає один `String`.
* `ChangeColor` ("змінити колір") включає три значення `i32`.

Визначення енума з варіантами, схожими на наведені у Блоці коду 6-2, нагадує визначення різних видів структур, але енум не використовує ключового слова `struct` і всі варіанти згруповані разом в одному типі `Message`. Наступні структури могли б зберігати ті самі дані, що й варіанти попереднього енума:

```rust
{{#rustdoc_include ../listings/ch06-enums-and-pattern-matching/no-listing-04-structs-similar-to-message-enum/src/main.rs:here}}
```

But if we used the different structs, each of which has its own type, we couldn’t as easily define a function to take any of these kinds of messages as we could with the `Message` enum defined in Listing 6-2, which is a single type.

Енуми та структури мають ще одну спільну рису: як за допомогою `impl` ми можемо оголошувати методи на структурах, ми можемо так само їх оголошувати на енумах. Ось метод, що зветься `call`, який можна визначити на нашому енумі `Message`:

```rust
{{#rustdoc_include ../listings/ch06-enums-and-pattern-matching/no-listing-05-methods-on-enums/src/main.rs:here}}
```

Тіло методу використає `self`, щоб отримати значення, для кого було викликано метод. У цьому прикладі ми створили змінну `m`, що має значення `Message::Write(String::from("hello"))`, і саме цей `self` буде в тілі методу `call`, коли буде виконано `m.call()`.

Let’s look at another enum in the standard library that is very common and useful: `Option`.

### Енум `Option` і його переваги над null-значеннями

Цей підрозділ розглядає використання `Option`, ще одного енума, визначеного в стандартній бібліотеці. Тип `Option` кодує дуже поширену ситуацію, де значення може бути чи його може не бути.

For example, if you request the first item in a non-empty list, you would get a value. If you request the first item in an empty list, you would get nothing. Те, що ця концепція виражена в системі типів, означає, що компілятор може перевірити, що ви обробили всі можливі варіанти, які потребують обробки; цей функціонал запобігає вкрай поширеним в інших мовах програмування вадам.

Дизайн мови програмування часто оцінюють за тим, який функціонал у ній є; але функціонал, який свідомо уникли, також важливий. Rust не має такої особливості, як null, що є в багатьох інших мовах. *Null* - це значення, що означає відсутність значення. У мовах із null змінні завжди можуть бути в одному з двох станів: null і не-null.

In his 2009 presentation “Null References: The Billion Dollar Mistake,” Tony Hoare, the inventor of null, has this to say:

> Я називаю це своєю помилкою на мільярд доларів. У той час я розробляв першу всеосяжну систему типів для посилань в об'єктноорієнтованій мові. Моєю метою було переконатися, що всі використання посилань будуть абсолютно безпечними, з автоматичною перевіркою компілятором. Але я не міг опиратися спокусі додати нульове посилання просто тому, що його було так легко реалізувати. Це призвело до незлічених помилок, вразливостей і системних збоїв, які, мабуть, коштували мільярд доларів болю і шкоди за останні 40 років.

Проблема з null-значеннями полягає в тому, що якщо ви спробуєте використовувати значення, яке є null, ніби це не null, ви дістанете помилку. А оскільки ця властивість є поширеною, стає неймовірно просто помилитися таким чином.

However, the concept that null is trying to express is still a useful one: a null is a value that is currently invalid or absent for some reason.

Проблема насправді не в самій концепції, а в конкретній реалізації. Відтак Rust не має null-значень, але має енум, що представляє концепцію присутнього чи відсутнього значення. Цей енум - `Option<T>`, і він [визначений у стандартній бібліотеці][option]<!-- ignore -->
ось так:

```rust
enum Option<T> {
  None,
  Some(T),
}
```

Enum `Option<T>` настільки корисний, що він включений у прелюдію; вам не потрібно явно вводити його в область видимості програми. Його варіанти також введені у прелюдію: ви можете використовувати `Some` та `None` напряму без префіксу `Option::`. Утім `Option<T>` - це лише звичайний енум, а `Some(T)` та `None` - лише варіанти типу `Option<T>`.

Запис `<T>` - особливість Rust, про яку ми ще не говорили. Це параметр узагальненого типу, і детальніше ми розглянемо узагальнення в Розділі 10. For now, all you need to know is that `<T>` means that the `Some` variant of the `Option` enum can hold one piece of data of any type, and that each concrete type that gets used in place of `T` makes the overall `Option<T>` type a different type. Here are some examples of using `Option` values to hold number types and string types:

```rust
{{#rustdoc_include ../listings/ch06-enums-and-pattern-matching/no-listing-06-option-examples/src/main.rs:here}}
```

Тип `some_number` - `Option<i32>`. Типом `some_char` є `Option<char>`, тобто інший тип. Rust може вивести ці типи, бо ми вказали значення всередині варіанту `Some`. А для `absent_number` Rust вимагає, щоб ми анотували весь тип `Option`: компілятор не може вивести тип відповідного варіанту `Some`, що міститиме значення, за самим лише значенням `None`. Тут ми вказуємо Rust, що хочемо, аби `absent_number` мав тип `Option<i32>`.

Коли у нас є значення `Some`, ми знаємо, що значення наявне, і значення зберігається в варіанті `Some`. When we have a `None` value, in some sense it means the same thing as null: we don’t have a valid value. So why is having `Option<T>` any better than having null?

Одним словом, оскільки `Option<T>` і `T` (де `T` може бути будь-яким типом) - різні типи, компілятор не дозволить нам використовувати значення `Option<T>` так, ніби ми маємо коректне значення. For example, this code won’t compile, because it’s trying to add an `i8` to an `Option<i8>`:

```rust,ignore,does_not_compile
{{#rustdoc_include ../listings/ch06-enums-and-pattern-matching/no-listing-07-cant-use-option-directly/src/main.rs:here}}
```

If we run this code, we get an error message like this one:

```console
{{#include ../listings/ch06-enums-and-pattern-matching/no-listing-07-cant-use-option-directly/output.txt}}
```

Сильно! Насправді це повідомлення про помилку означає, що Rust не розуміє, як додати `i8` та `Option<i8>`, оскільки вони різних типів. Коли у нас у Rust є значення типу на кшталт `i8`, компілятор гарантує, що у нас завжди є коректне значення. Ми можемо діяти впевнено без потреби у перевірці на null перш ніж використовувати це значення. Тільки тоді, коли у нас є `Option<i8>` (або будь-який тип чи значення, з яким ми працюємо), ми маємо турбуватися про те, що, можливо, значення не буде, і компілятор переконається, що ми обробляємо цей випадок, перш ніж використовувати значення.

Іншими словами, перед тим, як виконувати операції, які можна робити з `T`, треба перетворити значення `Option<T>` на `T`. Generally, this helps catch one of the most common issues with null: assuming that something isn’t null when it actually is.

Відсутність потреби турбуватися про некоректне припущення про не-null значення допомагає вам бути певнішим у власному коді. Щоб значення могло бути null, вам треба явно це вказати зробивши тип цього значення `Option<T>`. Потім, коли ви використовуєте це значення, від вас вимагається явно обробити випадок, коли це значення null. Всюди, де значення має тип, відмінний від `Option<T>`, ви *можете* безпечно припустити, що це значення не null. Це свідоме рішення при розробці Rust для обмеження передавання null і збільшення безпеки коду Rust.

So how do you get the `T` value out of a `Some` variant when you have a value of type `Option<T>` so that you can use that value? The `Option<T>` enum has a large number of methods that are useful in a variety of situations; you can check them out in [its documentation][docs]<!-- ignore -->. Becoming familiar with the methods on `Option<T>` will be extremely useful in your journey with Rust.

В цілому, щоб скористатися значенням `Option<T>`, ми хочемо мати код, що обробить обидва варіанти. Ми хочемо, щоб певний код виконувався лише для значень `Some(T)`, і цей код міг використовувати внутрішнє `T`. You want some other code to run only if you have a `None` value, and that code doesn’t have a `T` value available. The `match` expression is a control flow construct that does just this when used with enums: it will run different code depending on which variant of the enum it has, and that code can use the data inside the matching value.

[IpAddr]: ../std/net/enum.IpAddr.html
[option]: ../std/option/enum.Option.html
[docs]: ../std/option/enum.Option.html
