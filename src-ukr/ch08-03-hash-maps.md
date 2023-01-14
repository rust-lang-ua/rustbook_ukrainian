## Зберігання ключів і пов'язаних значень у хешмапах

Остання з поширених колекцій - це *хешмапа*. Тип `HashMap<K, V>` зберігає відображення ключів типу `K` на значення типу `V`, використовуючи *функцію хешування*, яка визначає, як розмістити ці ключі та значення у пам'яті. Багато мов програмування підтримують таку структуру даних, але часто використовують іншу назву, таку як хеш, відображення, хеш-таблиця, словник або асоціативний масив, це тільки декілька назв.

Хешмапи є корисними, коли ви хочете шукати дані не за індексом, як у векторах, а за допомогою ключа довільного типу. Наприклад, у грі ви можете відстежувати результат кожної команди за хешмапою, у якій кожен ключ є назвою команди, а значення є її рахунком. Маючи назву команди, ви можете отримати її рахунок.

У цьому розділі ми пройдемося базовим API хешмап, але багато інших корисностей ховаються у функціях, визначених на `HashMap<K, V>` у стандартній бібліотеці. Як завжди, зверніться до документації стандартної бібліотеки для додаткової інформації.

### Створення нової хешмапи

Один зі способів створення порожньої хешмапи - це застосувати `new` і додати елементи за допомогою `insert`. У Блоці коду 8-20 ми відстежуємо рахунки двох команд, що називаються *Синя* та *Жовта*. Синя команда починає з 10 очками, а Жовта - з 50.

```rust
{{#rustdoc_include ../listings/ch08-common-collections/listing-08-20/src/main.rs:here}}
```


<span class="caption">Блок коду 8-20: Створення нової хешмапи і вставлення деяких ключів та значень</span>

Зверніть увагу, що нам, по-перше, треба зробити `use` для `HashMap` з розділу колекцій стандартної бібліотеки. З трьох наших загальних колекцій ця використовується найрідше, тож вона не включена до функціонала, який автоматично додається до області видимості у прелюдії. Хешмапи також мають меншу підтримку від стандартної бібліотеки; скажімо, не існує вбудованого макросу для їхнього конструювання.

Так само як і вектори, хешмапи зберігають свої дані у купі. Цей `HashMap` має ключі типу `String` і значення типу `i32`. Як і вектори, хешмапи є однорідними: усі ключі мають бути одного і того ж самого типу, і всі значення мають бути одного типу.

### Доступ до значень у хешмапі

Ми можемо отримати значення з хешмари, надавши її ключ методу `get`, як показано у Блоці коду 8-21.

```rust
{{#rustdoc_include ../listings/ch08-common-collections/listing-08-21/src/main.rs:here}}
```


<span class="caption">Блок коду 8-21: доступ до рахунку Синьої команди, що зберігається у хешмапі</span>

Тут `score` буде мати значення, пов'язане з Синьою командою, і результат буде `10`. Метод `get` повертає `Option<&V>`; якщо у хешмапі для цього ключа немає відповідного значення, `get` поверне `None`. Ця програма обробляє `Option` викликом `unwrap_or`, щоб встановити `score` у нуль, якщо `scores` не має запису для цього ключа.

Ми можемо ітерувати по кожній парі ключ/значення в хешмапі схожим чином, як ми робимо з векторами, використовуючи цикл `for`:

```rust
{{#rustdoc_include ../listings/ch08-common-collections/no-listing-03-iterate-over-hashmap/src/main.rs:here}}
```

Цей код виведе кожну пару в довільному порядку:

```text
Yellow: 50
Blue: 10
```

### Хешмапи і володіння

Для типів, які реалізують трейт `Copy`, наприклад `i32`, значення копіюються до хешмапи. Для значень, які мають володіння, таких як `String`, значення будуть переміщені і хешмапа буде володіти цими значеннями, як це показано у Блоці коду 8-22.

```rust
{{#rustdoc_include ../listings/ch08-common-collections/listing-08-22/src/main.rs:here}}
```


<span class="caption">Блок коду 8-22: демонстрація, що ключі та значення є у володінні хешмапи після додавання</span>

Ми не можемо використовувати змінні `field_name` і `field_value` після переміщення в хешмапу за допомогою виклику `insert`.

Якщо ми вставляємо посилання на значення до хешмапи, ці значення не будуть переміщені до хешмапи. Значення, на які вказують посилання, мають бути коректними щонайменше стільки ж, скільки існує хешмапа. Ви поговоримо більше про ці справи у підрозділі ["Перевірка коректності посилань за допомогою часів існування"]()<!-- ignore --> Розділу 10.

### Оновлення хешмапи

Хоча кількість пар ключів і значень зростає, кожен унікальний ключ може мати тільки одне значення, пов’язане з ним, в кожен момент (але не навпаки: наприклад, команда Синя і Жовта могли мати значення 10, збережене в хешмапі `scores`).

Коли ви хочете змінити дані в хешмапі, вам необхідно вирішити, як обробляти випадок, коли ключ уже має присвоєне значення. Ви можете замінити старе значення на нове значення, повністю проігнорувавши старе значення. Ви можете зберегти старе значення і проігнорувати нове значення, і лише коли ключ *не має* значення, додавати нове. Або ж ви можете поєднати старе значення і нове значення. Подивімося, як зробити кожен варіант!

#### Перезапис значення

Якщо ми вставляємо ключ і значення до хешмапи і тоді вставляємо той самий ключ із іншим значенням, то значення, асоційоване з цим ключем, буде замінено. Попри те, що код у Блоці коду 8-23 викликає `insert` двічі, хешмапа міститиме лише одну пару ключ/значення, оскільки ми обидва рази вставляємо значення для ключа Синьої команди.

```rust
{{#rustdoc_include ../listings/ch08-common-collections/listing-08-23/src/main.rs:here}}
```


<span class="caption">Блок коду 8-23: заміна значення, збереженого з певним ключем</span>

Цей код виведе `{"Blue": 25}`. Початкове значення `10` було перезаписане.

<!-- Old headings. Do not remove or links may break. -->
<a id="only-inserting-a-value-if-the-key-has-no-value"></a>

#### Додавання ключа та значення тільки якщо ключ відсутній

Доволі поширено перевіряти, чи певний ключ уже присутній у хешмапі зі значенням, а тоді якщо ключ існує, то наявне значення має залишатися таким, яким воно є. Якщо ж ключ відсутній, вставити його і значення для нього.

Хешмапи мають спеціальне API для цього, що зветься `entry`, яке приймає параметром ключ, який ви хочете перевірити. Значення, що повертається з методу `entry` - це енум, що зветься `Entry`, який представляє значення, що може існувати або не існувати. Скажімо, ми хочемо перевірити, чи ключ для Жовтої команди має пов'язане з ним значення. Як ні, ми хочемо вставити значення 50, і те саме для Синьої команди. За допомогою API `entry`, код стає схожим на Блок коду 8-24.

```rust
{{#rustdoc_include ../listings/ch08-common-collections/listing-08-24/src/main.rs:here}}
```


<span class="caption">Блок коду 8-24: використання методу `entry` для вставляння лише якщо ключ ще не має відповідного значення</span>

Метод `or_insert` для `Entry` за визначенням повертає мутабельне посилання на відповідний ключ `Entry`, якщо ключ існує, а як ні, то вставляє параметр як нове значення для цього ключа і повертає мутабельне посилання на нове значення. Ця техніка набагато чистіша, ніж написання логіки самостійно і ще, крім того, краще працює з borrow checker.

Запуск коду у Блоці коду 8-24 надрукує `{"Yellow": 50, "Blue": 10}`. Перший виклик `entry` вставить ключ для Жовтої команди зі значенням 50, бо Жовта команда ще не має свого значення. Другий виклик `entry` не змінить хешмапу, бо Синя команда вже має значення 10.

#### Updating a Value Based on the Old Value

Another common use case for hash maps is to look up a key’s value and then update it based on the old value. For instance, Listing 8-25 shows code that counts how many times each word appears in some text. We use a hash map with the words as keys and increment the value to keep track of how many times we’ve seen that word. If it’s the first time we’ve seen a word, we’ll first insert the value 0.

```rust
{{#rustdoc_include ../listings/ch08-common-collections/listing-08-25/src/main.rs:here}}
```


<span class="caption">Listing 8-25: Counting occurrences of words using a hash map that stores words and counts</span>

This code will print `{"world": 2, "hello": 1, "wonderful": 1}`. You might see the same key/value pairs printed in a different order: recall from the [“Accessing Values in a Hash Map”][access]<!-- ignore --> section that iterating over a hash map happens in an arbitrary order.

The `split_whitespace` method returns an iterator over sub-slices, separated by whitespace, of the value in `text`. The `or_insert` method returns a mutable reference (`&mut V`) to the value for the specified key. Here we store that mutable reference in the `count` variable, so in order to assign to that value, we must first dereference `count` using the asterisk (`*`). The mutable reference goes out of scope at the end of the `for` loop, so all of these changes are safe and allowed by the borrowing rules.

### Hashing Functions

By default, `HashMap` uses a hashing function called *SipHash* that can provide resistance to Denial of Service (DoS) attacks involving hash tables[^siphash]<!-- ignore -->. This is not the fastest hashing algorithm available, but the trade-off for better security that comes with the drop in performance is worth it. If you profile your code and find that the default hash function is too slow for your purposes, you can switch to another function by specifying a different hasher. A *hasher* is a type that implements the `BuildHasher` trait. We’ll talk about traits and how to implement them in Chapter 10. You don’t necessarily have to implement your own hasher from scratch; [crates.io](https://crates.io/)<!-- ignore --> has libraries shared by other Rust users that provide hashers implementing many common hashing algorithms.

## Summary

Vectors, strings, and hash maps will provide a large amount of functionality necessary in programs when you need to store, access, and modify data. Here are some exercises you should now be equipped to solve:

* Given a list of integers, use a vector and return the median (when sorted, the value in the middle position) and mode (the value that occurs most often; a hash map will be helpful here) of the list.
* Convert strings to pig latin. The first consonant of each word is moved to the end of the word and “ay” is added, so “first” becomes “irst-fay.” Words that start with a vowel have “hay” added to the end instead (“apple” becomes “apple-hay”). Keep in mind the details about UTF-8 encoding!
* Using a hash map and vectors, create a text interface to allow a user to add employee names to a department in a company. For example, “Add Sally to Engineering” or “Add Amir to Sales.” Then let the user retrieve a list of all people in a department or all people in the company by department, sorted alphabetically.

The standard library API documentation describes methods that vectors, strings, and hash maps have that will be helpful for these exercises!

We’re getting into more complex programs in which operations can fail, so, it’s a perfect time to discuss error handling. We’ll do that next!
ch10-03-lifetime-syntax.html#validating-references-with-lifetimes

[^siphash]: [https://en.wikipedia.org/wiki/SipHash](https://en.wikipedia.org/wiki/SipHash)

[access]: #accessing-values-in-a-hash-map
