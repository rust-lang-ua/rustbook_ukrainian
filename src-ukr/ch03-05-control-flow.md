## Управління потоком виконання

Здатність виконувати чи ні певний код залежно від того, чи умова істинна, чи повторити певний код кілька разів, доки умова істинна - базові будівельні елементи коду у більшості мов програмування. Найпоширеніші конструкції, що дозволяють вам управляти потоком виконання коду на Rust є вирази `if` та цикли.

### Вирази `if`

Вираз `if` дозволяє розгалужувати код у залежності від умов. Ви задаєте умову, а потім вказуєте: “Якщо цю умову дотримано, запустити цей блок коду. Якщо ж умову не дотримано, не запускай цей блок коду”.

Створіть новий проект з назвою *branches* у вашій теці *projects* для вправ із виразом `if`. У файл *src/main.rs* введіть таке:

<span class="filename">Файл: src/main.rs</span>

```rust
{{#rustdoc_include ../listings/ch03-common-programming-concepts/no-listing-26-if-true/src/main.rs}}
```

Всі вирази `if` починаються з ключового слова `if`, за яким іде умова. В цьому випадку умова перевіряє, має чи ні змінна `number` значення, менше за 5. Ми розміщуємо блок коду, який виконається виконати, якщо умова істинна, одразу після умови в фігурних дужках. Блоки коду, прив'язані до умов у виразах `if`, іноді звуть *рукавами*, так само як рукави у виразах `match`, що ми обговорювали у підрозділі [Порівняння здогадки з таємним числом]()<!--
ignore --> Розділу 2.

Також можна додати необов'язковий вираз `else`, як ми зробили тут, щоб надати програмі альтернативний блок коду для виконання, якщо умова виявиться хибною. Якщо ви не надасте виразу `else`, а умова буде хибною, програма просто пропустить блок `if` і перейде до наступного фрагмента коду.

Спробуйте запустити цей код; ви маєте побачити, що він виведе таке:

```console
{{#include ../listings/ch03-common-programming-concepts/no-listing-26-if-true/output.txt}}
```

Let’s try changing the value of `number` to a value that makes the condition `false` to see what happens:

```rust,ignore
{{#rustdoc_include ../listings/ch03-common-programming-concepts/no-listing-27-if-false/src/main.rs:here}}
```

Запустіть програму знову і подивіться на вивід:

```console
{{#include ../listings/ch03-common-programming-concepts/no-listing-27-if-false/output.txt}}
```

Також варто зазначити, що умова в цьому коді *має* бути типу `bool`. Якщо умова не `bool`, ми отримаємо помилку. Наприклад, спробуйте запустити такий код:

<span class="filename">Файл: src/main.rs</span>

```rust,ignore,does_not_compile
{{#rustdoc_include ../listings/ch03-common-programming-concepts/no-listing-28-if-condition-must-be-bool/src/main.rs}}
```

The `if` condition evaluates to a value of `3` this time, and Rust throws an error:

```console
{{#include ../listings/ch03-common-programming-concepts/no-listing-28-if-condition-must-be-bool/output.txt}}
```

Помилка показує, що Rust очікував `bool`, але виявив ціле число. Rust не буде автоматично намагатися перетворити небулівські типи в булівський, на відміну від таких мов, як Ruby чи JavaScript. Ви маєте завжди явно надавати виразу `if` умову булівського типу. Якщо ми хочемо, щоб блок із кодом `if` виконувався тільки, скажімо, якщо число не дорівнює `0`, ми можемо змінити вираз `if` на такий:

<span class="filename">Файл: src/main.rs</span>

```rust
{{#rustdoc_include ../listings/ch03-common-programming-concepts/no-listing-29-if-not-equal-0/src/main.rs}}
```

Виконання цього коду виведе `number was something other than zero`.

#### Обробка множинних умов за допомогою `else if`

Можливо обирати з багатьох умов, комбінуючи `if` та `else` у ланцюжок виразів `else if`. Наприклад:

<span class="filename">Файл: src/main.rs</span>

```rust
{{#rustdoc_include ../listings/ch03-common-programming-concepts/no-listing-30-else-if/src/main.rs}}
```

Ця програма має чотири можливі шляхи. Після запуску, ви маєте побачити таке:

```console
{{#include ../listings/ch03-common-programming-concepts/no-listing-30-else-if/output.txt}}
```

Коли ця програма виконується, вона перевіряє по черзі кожен вираз `if` і виконує перший блок, для якого умова справджується. Зверніть увагу, що, хоча 6 і ділиться на 2, ми не бачимо повідомлення `number is divisible by 2`, так само як і `number is not divisible by 4, 3 чи 2` з блоку `else` - бо Rust виконає тільки той блок, в якого першого буде істинна умова, а знайшовши його, не перевіряє всю решту умов.

Забагато виразів `else if` можуть захарастити ваш код, тому, якщо вам треба більш ніж одна така конструкція, цілком можливо, що знадобиться рефакторизувати ваш код. У Розділі 6 описана потужна конструкція мови Rust для розгалуження, що зветься `match`, для таких випадків.

#### Використання `if` в інструкції `let`

Because `if` is an expression, we can use it on the right side of a `let` statement to assign the outcome to a variable, as in Listing 3-2.

<span class="filename">Файл: src/main.rs</span>

```rust
{{#rustdoc_include ../listings/ch03-common-programming-concepts/listing-03-02/src/main.rs}}
```


<span class="caption">Listing 3-2: Assigning the result of an `if` expression to a variable</span>

Змінна `number` буде прив'язана до значення, залежно від результату обчислення виразу `if`. Запустіть цей код і подивіться, що відбудеться:

```console
{{#include ../listings/ch03-common-programming-concepts/listing-03-02/output.txt}}
```

Нагадаємо, що значенням блоку коду є значення останнього виразу в них, а числа як такі самі є виразами. В цьому випадку, значення всього виразу `if` залежить від того, який блок коду буде виконано. Це означає, що значення, які можуть бути результатами у кожному рукаві `if` мають бути одного типу; у Блоці коду 3-2 результати рукавів `if` та `else` є цілими числами типу `i32`. Якщо ж типи не будуть збігатися, як у наступному прикладі, ми отримаємо помилку:

<span class="filename">Файл: src/main.rs</span>

```rust,ignore,does_not_compile
{{#rustdoc_include ../listings/ch03-common-programming-concepts/no-listing-31-arms-must-return-same-type/src/main.rs}}
```

Якщо ми спробуємо запустити цей код, то отримаємо помилку. Рукави `if` та `else` мають несумісні типи значень, і Rust точно вказує, де шукати проблему в програмі:

```console
{{#include ../listings/ch03-common-programming-concepts/no-listing-31-arms-must-return-same-type/output.txt}}
```

Вираз у блоці `if` обчислюється у ціле число, а вираз у блоці `else` обчислюється у стрічку. Це не працює, оскільки змінна мусить мати лише один тип. Rust має точно знати під час компіляції тип змінної `number`, щоб перевірити, що цей тип коректний усюди, де змінна `number` використовується. Rust не зможе зробити це, якщо тип `number` буде визначений після запуску програми; компілятор був би складнішим і надавав би менше гарантій стосовно коду, якби мусив стежити за чисельними можливими типами кожної змінної.

### Повторення коду за допомогою циклів

Часто трапляється, що блок коду треба виконати більше одного разу. Для цього Rust надає декілька *циклів*. Цикл виконує весь код тіла циклу до кінця, після чого починає спочатку. Для експериментів з циклами зробімо новий проєкт під назвою *loops*.

У Rust є три види циклів: `loop`, `while` та `for`. Спробуємо кожен з них.

#### Повторення коду за допомогою `loop`

The `loop` keyword tells Rust to execute a block of code over and over again forever or until you explicitly tell it to stop.

As an example, change the *src/main.rs* file in your *loops* directory to look like this:

<span class="filename">Файл: src/main.rs</span>

```rust,ignore
{{#rustdoc_include ../listings/ch03-common-programming-concepts/no-listing-32-loop/src/main.rs}}
```

Якщо запустити цю програму, ми побачимо, що `again!` виводиться неперервно раз у раз, доки ми не зупинимо програму вручну. Більшість терміналів підтримують клавіатурне скорочення <span class="keystroke">ctrl+c</span>, яке зупиняє програму, що застрягла у нескінченому циклі. Спробуйте:


<!-- manual-regeneration
cd listings/ch03-common-programming-concepts/no-listing-32-loop
cargo run
CTRL-C
-->

```console
$ cargo run
   Compiling loops v0.1.0 (file:///projects/loops)
    Finished dev [unoptimized + debuginfo] target(s) in 0.29s
     Running `target/debug/loops`
again!
again!
again!
again!
^Cagain!
```

Символ `^C` позначає, де ви натиснули <span class="keystroke">ctrl-c</span>. Слово `again!` може вивестися після `^C` чи ні, залежно від того, в який саме момент виконання коду був надісланий сигнал зупинки.

На щастя, Rust надає також інший, більш надійний спосіб перервати цикл за допомогою коду. Ви можете розмістити в циклі ключове слово `break`, щоб сказати програмі, коли припинити виконувати цикл. Згадайте, що ми вже його використовували у грі "Відгадай число" в підрозділі ["Вихід після вдалої здогадки"]()<!-- ignore
--> Розділу 2, щоб вийти з програми, коли користувач вигравав у грі, відгадавши правильне число.

We also used `continue` in the guessing game, which in a loop tells the program to skip over any remaining code in this iteration of the loop and go to the next iteration.

#### Повернення значень з циклів

Одне з застосувань циклу `loop` - повторні спроби здійснити операцію, яка може зазнати невдачі, такої як перевірка, чи завершив певний процес свою роботу. Вам може бути потрібно при цьому передати результат цієї операції з циклу для використання в подальшому коді. Для цього, ви можете дописати значення, що ви хочете повернути, після виразу `break`, яким ви зупиняєте цикл; це значення повернеться з циклу і ви зможете використати його, як показано тут:

```rust
{{#rustdoc_include ../listings/ch03-common-programming-concepts/no-listing-33-return-value-from-loop/src/main.rs}}
```

Перед циклом ми проголошуємо змінну `counter` і ініціалізуємо її в `0`. Потім ми проголошуємо змінну `result`, що отримає значення, повернуте з циклу. На кожній ітерації циклу, ми додаємо `1` до змінної `counter`, а потім перевіряємо, чи дорівнює counter `10`. Коли так, ми використовуємо ключове слово `break` зі значенням `counter * 2`. Після циклу ми ставимо крапку з комою, щоб завершити інструкцію, що присвоює значення змінній `result`. Наприкінці, ми виводимо значення `result`, у цьому випадку 20.

#### Мітки циклів для розрізнення між декількома циклами

Якщо у вас є цикли, вкладені в інші цикли, `break` і `continue` стосуються найглибшого циклу в точці, де вони застосовані. Ви можете за потреби вказати *мітку циклу* на циклі, що можна потім використати в інструкціях `break` та `continue`, щоб вказати, що ці ключові слова стосуються циклу з міткою, а не найглибшого циклу. Позначки циклу починаються з одинарної лапки. Ось приклад із двома вкладеними циклами:

```rust
{{#rustdoc_include ../listings/ch03-common-programming-concepts/no-listing-32-5-loop-labels/src/main.rs}}
```

Зовнішній цикл має позначку `'counting_up`, і він лічить від 0 до 2. Внутрішній цикл без позначки лічить навпаки від 10 до 9. Перший `break`, без указання мітки, виходить лише з внутрішнього циклу. Інструкція `break 'counting_up;` вийде з зовнішнього циклу. Цей код виведе:

```console
{{#rustdoc_include ../listings/ch03-common-programming-concepts/no-listing-32-5-loop-labels/output.txt}}
```

#### Умовні цикли за допомогою `while`

Програмі часто потрібно обчислювати умову в циклі. Доки умова істинна, цикл виконується. Коли умова припиняє бути істинною, можна викликати `break`, щоб зупинити цикл. Такий цикл можна реалізувати за допомогою комбінації `loop`, `if`, `else` та `break`; якщо бажаєте, можете спробувати зробити це зараз. Утім, цей шаблон настільки поширений, що Rust має вбудовану конструкцію для цього, що зветься циклом `while`. У Блоці коду 3-3 ми використовуємо `while`, щоб повторити програму тричі, зменшуючи кожного разу відлік, і потім, після циклу, вивести повідомлення і завершитися.

<span class="filename">Файл: src/main.rs</span>

```rust
{{#rustdoc_include ../listings/ch03-common-programming-concepts/listing-03-03/src/main.rs}}
```


<span class="caption">Listing 3-3: Using a `while` loop to run code while a condition holds true</span>

Ця конструкція мови усуває складні вкладені конструкції, які були б потрібні, якби ви використовували `loop`, `if`, `else` та `break`, і вона зрозуміліша. Поки умова істинна, код виконується; в іншому разі, виходить з циклу.

#### Цикл по колекції за допомогою `for`

Ви можете скористатися конструкцією `while`, щоб зробити цикл по елементах колекції, такої, як масив. Наприклад, цикл у Блоці коду 3-4 виводить кожен елемент масиву `a`.

<span class="filename">Файл: src/main.rs</span>

```rust
{{#rustdoc_include ../listings/ch03-common-programming-concepts/listing-03-04/src/main.rs}}
```


<span class="caption">Listing 3-4: Looping through each element of a collection using a `while` loop</span>

Тут код перелічує всі елементи в масиві. Він починає з індексу `0`, а потім повторює, доки не досягне останнього індексу масиву (тобто коли `index < 5` вже не буде істинним). Виконання цього коду виведе всі елементи масиву:

```console
{{#include ../listings/ch03-common-programming-concepts/listing-03-04/output.txt}}
```

Всі п'ять значень з масиву з'являються в терміналі, як і очікувалося. Хоча `index` досягне значення `5`, виконання циклу припиняється до спроби отримати шосте значення з масиву.

Але такий підхід вразливий до помилок; ми можемо викликати паніку в програмі некоректним індексом чи умовою продовження. Скажімо, якщо ви зміните визначення масиву `a` так, щоб він мав чотири елементи, і забудете змінити умову на `while index < 4`, це код викличе паніку. Також він повільний, оскільки компілятор додає код для перевірки коректності індексу кожного елементу на кожній ітерації циклу.

Як стислішу альтернативу можна використати цикл `for`, який виконує код для кожного елементу колекції. Цикл `for` виглядає так, як показано в Блоці коду 3-5.

<span class="filename">Файл: src/main.rs</span>

```rust
{{#rustdoc_include ../listings/ch03-common-programming-concepts/listing-03-05/src/main.rs}}
```


<span class="caption">Listing 3-5: Looping through each element of a collection using a `for` loop</span>

Запустивши цей код, ми побачимо такий самий вивід, як і в Блоці коду 3-4. Що важливіше, ми збільшили безпеку коду та усунули можливість помилок - тепер неможливо, що код перейде за кінець масиву чи завершиться зарано, пропустивши кілька значень.

Using the `for` loop, you wouldn’t need to remember to change any other code if you changed the number of values in the array, as you would with the method used in Listing 3-4.

Безпечність і лаконічність циклів `for` робить їх найпоширенішою конструкцією циклів у Rust. Навіть у ситуаціях, де треба виконати певний код визначену кількість разів, як у прикладі відліком в циклі `while` з Блоку коду 3-3, більшість растацеанців скористаються циклом `for`. Для цього треба буде скористатися типом `Range` ("діапазон"), який надається стандартною бібліотекою і генерує послідовно всі числа, починаючи з одного і закінчуючись перед іншим.

Here’s what the countdown would look like using a `for` loop and another method we’ve not yet talked about, `rev`, to reverse the range:

<span class="filename">Файл: src/main.rs</span>

```rust
{{#rustdoc_include ../listings/ch03-common-programming-concepts/no-listing-34-for-range/src/main.rs}}
```

Виглядає трохи краще, правда ж?

## Підсумок

Нарешті закінчили! Це був величенький розділ: ви вивчили змінні, скалярні та складені типи даних, функції, коментарі, вирази `if`, та ще цикли! Якщо ви хочете повправлятися з концепціями, обговореними у цьому розділі, спробуйте написати програми, що роблять таке:

* Конвертує температуру між шкалами Фаренгейта та Цельсія.
* Обчислює n-е число Фібоначчі.
* Print the lyrics to the Christmas carol “The Twelve Days of Christmas,” taking advantage of the repetition in the song.

When you’re ready to move on, we’ll talk about a concept in Rust that *doesn’t* commonly exist in other programming languages: ownership. ch02-00-guessing-game-tutorial.html#comparing-the-guess-to-the-secret-number ch02-00-guessing-game-tutorial.html#quitting-after-a-correct-guess
ch02-00-guessing-game-tutorial.html#comparing-the-guess-to-the-secret-number ch02-00-guessing-game-tutorial.html#quitting-after-a-correct-guess
