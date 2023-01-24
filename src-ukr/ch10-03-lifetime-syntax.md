## Перевірка коректності посилань за допомогою часів існування

Часи існування (lifetimes) є ще одним узагальненим типом даних, який ми вже використовували. Замість того щоб гарантувати, що тип має бажану поведінку, часи існування гарантують що посилання є валідним доти, доки воно може бути нам потрібним.

В частині [“Посилання і позичання”]()<!-- ignore --> четвертого розділу ми не згадали про те, що кожне посилання в Rust має свій *час існування*, який обмежує час протягом якого посилання є дійсним. В більшості випадків, часи існування є неявними (implicit) та виведеними (inferred), так само як і типи. Ми зобовʼязані додавати анотації лише у випадках коли можливий більше ніж один тип. Відповідно, ми мусимо додавати анотації до часів існування лише якщо останні можуть бути використані у кілька різних способів. Rust зобовʼязує нас анотувати звʼязки використовуючи узагальнені параметри часу існування, щоб впевнитися що посилання використані протягом часу виконання програми будуть коректними.

Додавання анотацій для часів існування не є поширеним в інших мовах програмування, тож може бути здаватися дещо складним для сприйняття. Попри те що в цьому розділі ми не розглядатимемо всі деталі часів існування, ми обговоримо їх найбільш загальні способи використання, щоб в подальшому у вас не виникало проблем зі сприйняттям цієї концепції.

### Запобігання висячим посиланням з використанням часів існування

Головною метою використання часів існування є запобігання *висячим посиланням*, які зберігають в памʼяті дані, котрі більше не будуть використані програмою. Розглянемо приклад з блоку коду 10-16, який має внутрішню і зовнішню область видимості.

```rust,ignore,does_not_compile
{{#rustdoc_include ../listings/ch10-generic-types-traits-and-lifetimes/listing-10-16/src/main.rs}}
```


<span class="caption">Блок коду 10-16: Спроба використати посилання, значення якого лежить за межами області видимості</span>

> Примітка: Приклади у Блоках коду 10-16, 10-17 та 10-23 проголошують змінні без надання їм початкового значення, тож, назва змінної існує в зовнішньої області видимості. На перший погляд, може видатися, що це суперечить тому, що Rust не має нульових значень. Однак, якщо ми спробуємо використати змінну перед наданням їй значення, ми отримаємо помилку часу компіляції, що показує, що Rust дійсно не допускає значення null.

Зовнішня область видимості проголошує змінну з назвою `r` без початкового значення, а внутрішня область видимості проголошує змінну з назвою `x` з початковим значенням 5. Усередині внутрішньої області видимості ми намагаємося встановити значення r у посилання до `x`. Тоді внутрішня область видимості закінчується, і ми намагаємося вивести значення `r`. Цей код не компілюється, бо значення, на яке посилається `r`, вийшло з області видимості до того, як ми спробували ним скористатися. Ось повідомлення про помилку:

```console
{{#include ../listings/ch10-generic-types-traits-and-lifetimes/listing-10-16/output.txt}}
```

Змінна `x` не "існує достатньо довго". Причина в тому, що `x` вийде з області видимості коли внутрішня область видимості скінчиться у рядку 7. Але змінна `r` все ще валідна у зовнішній області видимості; оскільки її область видимості більша, ми кажемо, що вона "існує довше". Якби Rust дозволив цьому коду працювати, `r` посилався би на пам'ять, що була звільнена, коли `x` вийшов з області видимості, і все, що ми намагатимемося робити з `r`, не працюватиме належним чином. То як Rust визначає, що код є некоректним? Вона використовує borrow checker.

### Borrow Checker

Компілятор Rust має *borrow checker*, який порівнює області видимості і визначає, чи всі позичання валідні. Блок коду 10-17 показує такий самий код, як у Блоці коду 10-16, але з анотаціями, які показують часи існування змінних.

```rust,ignore,does_not_compile
{{#rustdoc_include ../listings/ch10-generic-types-traits-and-lifetimes/listing-10-17/src/main.rs}}
```


<span class="caption">Блок коду 10-17: анотації часів існування `r` та `x`, що називаються відповідно `'a` та `'b`</span>

Тут ми анотували час існування `r` як `'a` і час існування `x` як `'b`. Як бачите, час існування внутрішнього блоку `'b` є набагато меншим, ніж зовнішнього `'a`. Під час компіляції Rust порівнює розмір двох часів існування і бачить, що `r` має час існування `'a`, але він посилається на пам'ять з часом існування `'b`. Програма буде відхилена через те, що `'b` коротший за `'a`: те, на що посилаються, існує менше, ніж посилання.

Блок коду 10-18 виправляє код, щоб не було висячого посилання, і компілюється без жодних помилок.

```rust
{{#rustdoc_include ../listings/ch10-generic-types-traits-and-lifetimes/listing-10-18/src/main.rs}}
```


<span class="caption">Блок коду 10-18: посилання є валідним, бо дані мають довший час існування, ніж посилання</span>

Тут `x` має час існування `'b`, що у цьому випадку більше, ніж `'a`. Це означає, що `r` може посилатись на `x`, тому що Rust знає, що посилання в `r` завжди буде дійсним, поки `x` є дійсним.

Тепер, коли ви знаєте, де знаходяться часи існування посилань і як Rust аналізує часи існування, щоб гарантувати, що посилання завжди будуть валідними, розгляньмо узагальнені часи існування параметрів та значень, що повертаються, в контексті функцій.

### Узагальнені часи існування у функціях

Ми напишемо функцію, яка повертає довший з двох стрічкових слайсів. Ця функція прийматиме два стрічкові слайси і повертатиме один слайс. Після реалізації функції `longest`, код у Блоці коду 10-19 має вивести `The longest string is abcd`.

<span class="filename">Filename: src/main.rs</span>

```rust,ignore
{{#rustdoc_include ../listings/ch10-generic-types-traits-and-lifetimes/listing-10-19/src/main.rs}}
```


<span class="caption">Блок коду 10-19: функція `main`, що викликає функцію `longest` щоб знайти довший з двох стрічкових слайсів</span>

Зверніть увагу, що нам потрібно, щоб функція приймала рядки фрагментів, які є посиланнями, а не стрічки, тому що ми не хочемо, щоб функція `longest` брала володіння над своїми параметрами. Зверніться до підрозділу ["Стрічкові слайси як параметри"]()<!-- ignore --> Розділу 4 для детальнішого обговорення, чому ми хочемо використати саме такі параметри в Блоці коду 10-19.

Якщо ми спробуємо реалізувати функцію `longest` як показано у Блоці коду 10-20, вона не скомпілюється.

<span class="filename">Файл: src/lib.rs</span>

```rust,ignore,does_not_compile
{{#rustdoc_include ../listings/ch10-generic-types-traits-and-lifetimes/listing-10-20/src/main.rs:here}}
```


<span class="caption">Блок коду 10-20: реалізація функції `longest`, що повертає довший з двох стрічкових слайсів, але ще не компілюється</span>

Натомість ми отримуємо наступну помилку, яка говорить про часи існування:

```console
{{#include ../listings/ch10-generic-types-traits-and-lifetimes/listing-10-20/output.txt}}
```

Текст підказки показує, що тип, який повертається, потребує для себе вказаного узагальненого часу існування, оскільки Rust не може сказати, буде повернуте посилання `x` чи `y`. Насправді ми також цього не знаємо, оскільки блок `if` у тілі цієї функції повертає посилання на `x`, а блок `else` повертає посилання на `y`!

Коли ми визначаємо цю функцію, ми не знаємо конкретних значень, які будуть передані в цю функцію, тому ми не знаємо, спрацює випадок `if` чи `else`. Ми також не знаємо конкретного часу існування посилань, які будуть передані, тож ми не можемо подивитися на область видимості, як ми робили у Блоках коду 10-17 та 10-18, щоб визначити, чи посилання, яке ми повертаємо, буде завжди валідним. Borrow checker також не може визначити цього, оскільки він не знає як час існування `x` або `y` стосується часу існування значення, що повертається. Щоб виправити цю помилку, ми додамо узагальнені параметри часу існування, які визначають зв'язок між посиланнями, щоб borrow checker міг його проаналізувати.

### Синтаксис анотацій часу існування

Анотації часу існування не змінюють як довго існує посилання. Натомість вони описують взаємозв'язок часів існування багатьох посилань між собою, не впливаючи на ці часи існування. Так само як функції можуть приймати будь-який тип, коли сигнатура визначає параметр узагальненого типу, функції можуть приймати посилання з будь-яким часом існування, якщо заданий узагальнений параметр часу існування.

Анотації часу існування мають дещо незвичний синтаксис: імена параметрів часу існування мають починатися на апостроф (`'`) і зазвичай в нижньому регістрі та дуже короткі, як узагальнені типи. Більшість людей використовують ім'я `'a` для першої анотації часу існування. Ми розміщуємо анотації часу існування параметрів після `&` посилання, використовуючи пробіл для відокремлення анотації від типу посилання.

Ось кілька прикладів: посилання на `i32` без параметру часу існування, посилання на на `i32`, що має параметр часу існування `'a`, і мутабельне посилання на `i32`, що також має час існування `'a`.

```rust,ignore
&i32        // посилання
&'a i32     // посилання з явним часом існування
&'a mut i32 // мутабельне посилання з явним часом існування
```

Сам по собі одна анотація часу існування не має великого значення. оскільки анотації мають повідомляти Rust, як узагальнені параметри часу існування багатьох посилань співвідносяться один з одним. Дослідімо, як анотації часу існування співвідносяться одна з одною в контексті функції `longest`.

### Анотації часу існування у сигнатурах функцій

Щоб використовувати анотації часу існування в сигнатурах функцій, ми маємо проголосити узагальнений параметр *часу існування* в кутових дужках між назвою функції і списком параметрів, так, як це робиться з узагальненими параметрами *типу*.

Ми хочемо, щоб сигнатура виражала наступне обмеження: повернуте посилання буде валідним стільки, скільки обидва параметри будуть валідними. Це відношення між часом існування параметрів та значення, що повертається. Ми назвемо час існування `'a`, а тоді додамо його до кожного посилання, як показано в Блоці коду 10-21.

<span class="filename">Файл: src/main.rs</span>

```rust
{{#rustdoc_include ../listings/ch10-generic-types-traits-and-lifetimes/listing-10-21/src/main.rs:here}}
```


<span class="caption">Блок коду 10-21: Визначення функції `longest` із зазначенням, що всі посилання у сигнатурі повинні мати однаковий час існування `'a`</span>

Цей код має компілюватися і створювати бажаний результат, коли ми використовуємо його у функції `main` у Блоці коду 10-19.

Сигнатура функції тепер повідомляє Rust, що для якогось часу існування `'a` функція приймає два параметри, обидва з яких - стрічкові слайси, які існують принаймні так довго, як час існування `'a`. Сигнатура функції також каже Rust, що стрічковий слайс, повернутий з функції, існуватиме щонайменше так довго, як час існування `'a`. На практиці це означає, що час існування посилання, повернутого функцією `longest`, дорівнює коротшому з часів існування значень, на які посилаються аргументи функції. Ці відносини - це те, що ми хочемо, щоб Rust використовувала під час аналізу цього коду.

Пам'ятайте, що коли ми зазначаємо час існування параметрів на сигнатурі цієї функції, ми не змінюємо час існування жодного значення, які передаються або повертаються. Радше ми уточнюємо, що borrow checker повинен відкидати будь-які значення, які не відповідають цим обмеженням. Зверніть увагу, що функція `longest` не повинна точно знати, як довго `x` і `y` будуть існувати, лише те, що деяка область видимості може бути замінена на `'a` що задовольняє цій сигнатурі.

При анотації часів існування у функції анотації зазначаються у сигнатурі функції, а не в її тілі. Анотації часу існування стають частиною контракту функції, так само, як типи у сигнатурі. Те, що сигнатури функцій містять контракт на час існування, означає, що аналіз, який робить компілятор Rust, буде простішим. Якщо є проблема з тим, як функція анотована або як її викликають, помилки компілятора можуть точніше вказувати на частину нашого коду та обмеження. Якщо натомість компілятор Rust робив більше припущень про те, якими ми хотіли б бачити відносини між часами існування, компілятор міг би вказувати лише на використання нашого коду за багато кроків від причини проблеми.

Коли ми передаємо конкретні посилання до `longest`, конкретний час існування, що підставляється в `'a`, є частиною області видимості `x`, що перетинається з областю видимості `y`. Іншими словами, узагальнений час існування `'a` отримає конкретний час існування, що є рівним меншому з часів існування `x` та `y`. Оскільки ми анотували посилання, що повертається, тим самим параметром часу існування `'a`, посилання, що повертається, також буде валідним під час тривалості меншого з часів інсування `x` та `y`.

Подивімося, як анотації часу існування обмежують функцію `longest`, передаючи посилання, що мають різне конкретні часи існування. Блок коду 10-22 надає прямолінійний приклад.

<span class="filename">Файл: src/main.rs</span>

```rust
{{#rustdoc_include ../listings/ch10-generic-types-traits-and-lifetimes/listing-10-22/src/main.rs:here}}
```


<span class="caption">Блок коду 10-22: використання функції "longest" з посиланнями на значення "String", які мають різний конкретний час існування</span>

У цьому прикладі `string1` буде валідним до кінця зовнішньої області видимості, `string2` до кінця внутрішньої області видимості, а `result` посилається на щось, що є валідним до кінця внутрішньої області видимості. Запустіть цей код, і ви побачите, що borrow checker його приймає; код скомпілюється і виведе `The longest string
is long string is long`.

Далі розгляньмо приклад, який показує, що час існування посилання в `result` має бути меншим з тривалостей існування двох аргументів. Ми перемістимо оголошення змінної `result` за межі внутрішньої області видимості, але залишимо присвоєння значення змінній `result` усередині області видимості зі `string2`. Потім ми перемістимо `println!`, який використовує `result`, за межі внутрішньої області видимості, її після закінчення. Код в Блоці коду 10-23 не скомпілюється.

<span class="filename">Файл: src/main.rs</span>

```rust,ignore,does_not_compile
{{#rustdoc_include ../listings/ch10-generic-types-traits-and-lifetimes/listing-10-23/src/main.rs:here}}
```


<span class="caption">Блок коду 10-23: спроба використати `result` після виходу `string2` з області видимості</span>

Якщо ми спробуємо скомпілювати цей код, то отримаємо таку помилку:

```console
{{#include ../listings/ch10-generic-types-traits-and-lifetimes/listing-10-23/output.txt}}
```

Помилка показує, що для того, що `result` був валідним для інструкції `println!`, `string2` має бути валідним до кінця зовнішньої області видимості. Rust знає це, бо ми анотували час існування параметрів функції та значення, що повертається, за допомогою того самого параметра часу існування `'a`.

Як люди, ми можемо подивитися на цей код і побачити, що `string1` довша за `string2` і тому `result` міститиме посилання на `string1`. Оскільки `string1` ще не вийшла з області видимості, посилання на `string1` все ще буде валідним для інструкції `println!`. Однак, компілятор не може побачити, що посилання в цьому випадку валідне. Ми повідомили Rust, що час існування посилання, що повертається функцією `longest`, такий самий, як менший з часів існування переданих їй посилань. Тому borrow checker забороняє код у Блоці коду 10-23 як такий, що потенційно містить неправильне посилання.

Спробуйте провести більше експериментів, які змінюють значення і часи існування посилань, переданих у функцію `longest`, а також використання посилання, що повертається. Робіть припущення про те, чи пройдуть ваші експерименти borrow checker до компіляції; потім перевірте, щоб побачити, чи маєте ви рацію!

### Мислення в термінах часів існування

Спосіб, яким треба позначати параметри часу існування, залежить від того, що саме робить ваша функція. Наприклад, якби ми змінили реалізацію функції `longest`, щоб та завжди повертала перший параметр замість найдовшої стрічки, то нам не потрібно було б вказувати час існування для параметра `y`. Цей код буде скомпілюється:

<span class="filename">Файл: src/main.rs</span>

```rust
{{#rustdoc_include ../listings/ch10-generic-types-traits-and-lifetimes/no-listing-08-only-one-reference-with-lifetime/src/main.rs:here}}
```

Ми зазначили параметр часу існування `'a` для параметра `x` та типу, що повертається, але не для параметра `y`, оскільки час існування `у` у не має стосунку до часу існування `x` або значення, що повертається.

При поверненні посилання з функції, параметр часу існування для типу, що повертається, має збігатися з параметром часу існування для одного з параметрів. Якщо повернуте посилання *не* посилається на один з параметрів, воно повинне посилатись на значення, створене всередині цієї функції. Однак це буде висяче посилання, тому що значення вийде з області видимості в кінці функції. Розглянемо спробу реалізації функції `longest`, яка не компілюється:

<span class="filename">Файл: src/main.rs</span>

```rust,ignore,does_not_compile
{{#rustdoc_include ../listings/ch10-generic-types-traits-and-lifetimes/no-listing-09-unrelated-lifetime/src/main.rs:here}}
```

Тут, хоча ми й зазначили параметр часу існування `'a` для типу, що повертається, ця реалізація не буде скомпільованою, оскільки час існування значення, що повертається, взагалі не пов'язаний з часом існування параметрів. Ось яке повідомлення про помилку ми отримаємо:

```console
{{#include ../listings/ch10-generic-types-traits-and-lifetimes/no-listing-09-unrelated-lifetime/output.txt}}
```

Проблема в тому, що `result` виходить з області видимості й очищується в кінці функції `longest`. Ми також намагаємося повернути з функції посилання на `result`. Неможливо вказати параметри часу існування, які змінять висяче посилання, і Rust не дозволяє нам створити висяче посилання. У цьому випадку найкращим виправленням було б повернути тип, що володіє даними, замість посилання, щоб функція, яка викликала, була б відповідальною за очищення значення.

Зрештою, синтаксис часу існування стосується зв'язування часів існування різних параметрів і значень, що повертаються з функцій. Коли вони пов'язуються, Rust має достатньо інформації, щоб дозволити безпечні операції з пам'яттю і забороняти операції, які створювали б висячі вказівники або іншим чином порушували безпеку пам’яті.

### Анотації часів існування у визначеннях структур

Досі всі визначені нами структури містили типи, що володіють даними. Ми можемо визначити структури, що міститимуть посилання, але в цьому разі ми маємо додати анотацію часу існування до кожного посилання у визначенні структури. Блок коду 10-24 демонструє структуру, що зветься `ImportantExcerpt`, що містить стрічковий слайс.

<span class="filename">Файл: src/main.rs</span>

```rust
{{#rustdoc_include ../listings/ch10-generic-types-traits-and-lifetimes/listing-10-24/src/main.rs}}
```


<span class="caption">Блок коду 10-24: структура, що містить посилання, яке потребує анотації часу існування</span>

Ця структура має єдине поле `part`, що містить стрічковий слайс, що є посиланням. Як і для узагальнених типів даних, ми проголошуємо назву узагальненого параметру часу існування в кутових дужках після назви структури, щоб ми могли використати цей параметр часу існування у визначенні структури. Ця анотація означає, що екземпляр `ImportantExcerpt` не може існувати довше, ніж посилання, яке він містить у своєму полі `part`.

Функція `main` тут створює екземпляр структури `ImportantExcerpt`, який містить посилання на перше речення `String`, якою володіє змінна `novel`. Дані в `novel` існують до створення екземпляра `ImportantExcerpt`. Крім того, `novel` не виходить з області видимості, доки `ImportantExcerpt` не вийде з області видимості, тож посилання в екземплярі `ImportantExcerpt` є валідним.

### Елізія часу існування

Ви дізналися, що кожне посилання має час існування і ви маєте зазначити параметри часу існування для функцій та структур, які використовують посилання. Однак у Розділі 4 у нас була функція в Блоці коду 4-9, показана знову у Блоці коду 10-25, яка скомпілювалася без анотацій часу існування.

<span class="filename">Файл: src/lib.rs</span>

```rust
{{#rustdoc_include ../listings/ch10-generic-types-traits-and-lifetimes/listing-10-25/src/main.rs:here}}
```


<span class="caption">Блок коду 10-25: функція, визначена в Блоці коду 4-9, яка компілюється без анотацій часу існування, хоча параметр і тип, що повертається є посиланнями</span>

Причина, чому ця функція компілюється без анотацій часу існування, є історичною: у ранніх версіях (до 1.0) Rust цей код не скомпілювався б тому, що кожне посилання потребувало явного часу існування. На той момент сигнатура функції була б записана так:

```rust,ignore
fn first_word<'a>(s: &'a str) -> &'a str {
```

Після написання великої кількості коду на Rust, команда Rust виявила, що програмісти Rust вводять ті самі анотації часу існування знову і знову в конкретних випадках. Ці ситуації були передбачуваними та відповідали декільком визначеним шаблонів. Розробники запрограмували ці шаблони у код компілятора, щоб borrow checker міг вивести часи існування в цих ситуаціях і не потребував явних анотацій.

Цей шматок історії Rust має значення, оскільки цілком можливо, що буде виявлено ще більше визначених шаблонів і будуть додані до компілятора. Можливо, у майбутньому буде потрібно ще менше анотацій часу існування.

Шаблони, запрограмовані в аналіз посилань Rust, називаються *правилами елізії часів існування*. Це не правила для програмістів; вони є набором певних випадків, які розглядає компілятор, і якщо ваш код відповідає цим випадкам, вам не потрібно явно вказувати часи існування.

The elision rules don’t provide full inference. If Rust deterministically applies the rules but there is still ambiguity as to what lifetimes the references have, the compiler won’t guess what the lifetime of the remaining references should be. Instead of guessing, the compiler will give you an error that you can resolve by adding the lifetime annotations.

Lifetimes on function or method parameters are called *input lifetimes*, and lifetimes on return values are called *output lifetimes*.

The compiler uses three rules to figure out the lifetimes of the references when there aren’t explicit annotations. The first rule applies to input lifetimes, and the second and third rules apply to output lifetimes. If the compiler gets to the end of the three rules and there are still references for which it can’t figure out lifetimes, the compiler will stop with an error. These rules apply to `fn` definitions as well as `impl` blocks.

The first rule is that the compiler assigns a lifetime parameter to each parameter that’s a reference. In other words, a function with one parameter gets one lifetime parameter: `fn foo<'a>(x: &'a i32)`; a function with two parameters gets two separate lifetime parameters: `fn foo<'a, 'b>(x: &'a i32,
y: &'b i32)`; and so on.

The second rule is that, if there is exactly one input lifetime parameter, that lifetime is assigned to all output lifetime parameters: `fn foo<'a>(x: &'a i32)
-> &'a i32`.

The third rule is that, if there are multiple input lifetime parameters, but one of them is `&self` or `&mut self` because this is a method, the lifetime of `self` is assigned to all output lifetime parameters. This third rule makes methods much nicer to read and write because fewer symbols are necessary.

Let’s pretend we’re the compiler. We’ll apply these rules to figure out the lifetimes of the references in the signature of the `first_word` function in Listing 10-25. The signature starts without any lifetimes associated with the references:

```rust,ignore
fn first_word(s: &str) -> &str {
```

Then the compiler applies the first rule, which specifies that each parameter gets its own lifetime. We’ll call it `'a` as usual, so now the signature is this:

```rust,ignore
fn first_word<'a>(s: &'a str) -> &str {
```

The second rule applies because there is exactly one input lifetime. The second rule specifies that the lifetime of the one input parameter gets assigned to the output lifetime, so the signature is now this:

```rust,ignore
fn first_word<'a>(s: &'a str) -> &'a str {
```

Now all the references in this function signature have lifetimes, and the compiler can continue its analysis without needing the programmer to annotate the lifetimes in this function signature.

Let’s look at another example, this time using the `longest` function that had no lifetime parameters when we started working with it in Listing 10-20:

```rust,ignore
fn longest(x: &str, y: &str) -> &str {
```

Let’s apply the first rule: each parameter gets its own lifetime. This time we have two parameters instead of one, so we have two lifetimes:

```rust,ignore
fn longest<'a, 'b>(x: &'a str, y: &'b str) -> &str {
```

You can see that the second rule doesn’t apply because there is more than one input lifetime. The third rule doesn’t apply either, because `longest` is a function rather than a method, so none of the parameters are `self`. After working through all three rules, we still haven’t figured out what the return type’s lifetime is. This is why we got an error trying to compile the code in Listing 10-20: the compiler worked through the lifetime elision rules but still couldn’t figure out all the lifetimes of the references in the signature.

Because the third rule really only applies in method signatures, we’ll look at lifetimes in that context next to see why the third rule means we don’t have to annotate lifetimes in method signatures very often.

### Lifetime Annotations in Method Definitions

When we implement methods on a struct with lifetimes, we use the same syntax as that of generic type parameters shown in Listing 10-11. Where we declare and use the lifetime parameters depends on whether they’re related to the struct fields or the method parameters and return values.

Lifetime names for struct fields always need to be declared after the `impl` keyword and then used after the struct’s name, because those lifetimes are part of the struct’s type.

In method signatures inside the `impl` block, references might be tied to the lifetime of references in the struct’s fields, or they might be independent. In addition, the lifetime elision rules often make it so that lifetime annotations aren’t necessary in method signatures. Let’s look at some examples using the struct named `ImportantExcerpt` that we defined in Listing 10-24.

First, we’ll use a method named `level` whose only parameter is a reference to `self` and whose return value is an `i32`, which is not a reference to anything:

```rust
{{#rustdoc_include ../listings/ch10-generic-types-traits-and-lifetimes/no-listing-10-lifetimes-on-methods/src/main.rs:1st}}
```

The lifetime parameter declaration after `impl` and its use after the type name are required, but we’re not required to annotate the lifetime of the reference to `self` because of the first elision rule.

Here is an example where the third lifetime elision rule applies:

```rust
{{#rustdoc_include ../listings/ch10-generic-types-traits-and-lifetimes/no-listing-10-lifetimes-on-methods/src/main.rs:3rd}}
```

There are two input lifetimes, so Rust applies the first lifetime elision rule and gives both `&self` and `announcement` their own lifetimes. Then, because one of the parameters is `&self`, the return type gets the lifetime of `&self`, and all lifetimes have been accounted for.

### The Static Lifetime

One special lifetime we need to discuss is `'static`, which denotes that the affected reference *can* live for the entire duration of the program. All string literals have the `'static` lifetime, which we can annotate as follows:

```rust
let s: &'static str = "I have a static lifetime.";
```

The text of this string is stored directly in the program’s binary, which is always available. Therefore, the lifetime of all string literals is `'static`.

You might see suggestions to use the `'static` lifetime in error messages. But before specifying `'static` as the lifetime for a reference, think about whether the reference you have actually lives the entire lifetime of your program or not, and whether you want it to. Most of the time, an error message suggesting the `'static` lifetime results from attempting to create a dangling reference or a mismatch of the available lifetimes. In such cases, the solution is fixing those problems, not specifying the `'static` lifetime.

## Generic Type Parameters, Trait Bounds, and Lifetimes Together

Let’s briefly look at the syntax of specifying generic type parameters, trait bounds, and lifetimes all in one function!

```rust
{{#rustdoc_include ../listings/ch10-generic-types-traits-and-lifetimes/no-listing-11-generics-traits-and-lifetimes/src/main.rs:here}}
```

This is the `longest` function from Listing 10-21 that returns the longer of two string slices. But now it has an extra parameter named `ann` of the generic type `T`, which can be filled in by any type that implements the `Display` trait as specified by the `where` clause. This extra parameter will be printed using `{}`, which is why the `Display` trait bound is necessary. Because lifetimes are a type of generic, the declarations of the lifetime parameter `'a` and the generic type parameter `T` go in the same list inside the angle brackets after the function name.

## Summary

We covered a lot in this chapter! Now that you know about generic type parameters, traits and trait bounds, and generic lifetime parameters, you’re ready to write code without repetition that works in many different situations. Generic type parameters let you apply the code to different types. Traits and trait bounds ensure that even though the types are generic, they’ll have the behavior the code needs. You learned how to use lifetime annotations to ensure that this flexible code won’t have any dangling references. And all of this analysis happens at compile time, which doesn’t affect runtime performance!

Believe it or not, there is much more to learn on the topics we discussed in this chapter: Chapter 17 discusses trait objects, which are another way to use traits. There are also more complex scenarios involving lifetime annotations that you will only need in very advanced scenarios; for those, you should read the [Rust Reference][reference]. But next, you’ll learn how to write tests in Rust so you can make sure your code is working the way it should.
ch04-02-references-and-borrowing.html#references-and-borrowing ch04-03-slices.html#string-slices-as-parameters

[reference]: ../reference/index.html
