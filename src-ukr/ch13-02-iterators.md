## Обробка послідовностей елементів за допомогою ітераторів

Шаблон ітератора дозволяє вам виконати певну задачу з послідовністю елементів по черзі. Ітератор відповідає за логіку ітерації по елементах і визначення, коли послідовність закінчується. Коли ви користуєтесь ітераторами, вам не треба самостійно реалізовувати цю логіку.

У Rust ітератори є *лінивими*, тобто вони нічого не роблять до того моменту, коли ви викличете метод, що поглине ітератор і використає його. Наприклад, код у Блоці коду 13-10 створює ітератор по елементах вектора `v1`, викликавши метод `iter`, визначений для `Vec<T>`. Цей код як такий не робить нічого корисного.

```rust
{{#rustdoc_include ../listings/ch13-functional-features/listing-13-10/src/main.rs:here}}
```

<span class="caption">Блок коду 13-10: створення ітератора</span>

Ітератор зберігається у змінній `v1_iter`. Після того, як ми створили ітератор, ми можемо використовувати його у різні способи. У Блоці коду 3-5 з Розділу 3 ми ітерували по масиву за допомогою циклу `for`, щоб виконати певний код на кожному елементі. Під капотом тут неявно був створений і поглинутий ітератор, але до цього часу ми не звертали уваги на те, як саме це працює.

У прикладі з Блоку коду 13-11 ми відокремлюємо створення ітератора від його використання в циклі `for`. Коли цикл `for` викликають з ітератором у `v1_iter`, кожен елемент у ітераторі використовується одній ітерації циклу, який виводить кожне значення.

```rust
{{#rustdoc_include ../listings/ch13-functional-features/listing-13-11/src/main.rs:here}}
```

<span class="caption">Блок коду 13-11: використання ітератора у циклі `for`</span>

In languages that don’t have iterators provided by their standard libraries, you would likely write this same functionality by starting a variable at index 0, using that variable to index into the vector to get a value, and incrementing the variable value in a loop until it reached the total number of items in the vector.

Ітератори обробляють всю цю логіку за вас, скорочуючи код, який ви потенційно можете зіпсувати. Ітератори дають вам більше гнучкості для використання тієї ж логіки з різними типами послідовностей, а не лише структурами даних, які ви можете індексувати, такими як вектори. Розгляньмо, як ітератори це роблять.

### Трейт `Iterator` і метод `next`

Усі ітератори реалізують трейт, що зветься `Iterator`, визначений у стандартній бібліотеці. Визначення цього трейту виглядає ось так:

```rust
pub trait Iterator {
    type Item;

    fn next(&mut self) -> Option<Self::Item>;

    // методи зі стандартною реалізацією пропущені
}
```

Зверніть увагу, що це визначення використовує новий синтаксис: `type Item` і `Self::Item`, які визначають *асоційований тип* цього трейта. Ми глибше поговоримо про асоційовані типи у Розділі 19. Поки що, все, що вам слід знати - це те, що цей код каже, що реалізація трейту `Iterator` також вимагає, щоб ви визначили тип `Item`, і цей тип `Item` використовується як тип, що повертається методом `next`. Іншими словами, тип `Item` буде типом, повернутим з ітератора.

Трейт `Iterator` потребує від того, хто його реалізовує, визначення лише одного методу: методу `next`, який повертає за раз один елемент ітератора, обгорнутий у `Some` і, коли ітерація закінчиться, повертає `None`.

Ми можемо викликати метод `next` для ітераторів безпосередньо; Блок коду 13-12 демонструє, які значення повертаються повторюваними викликами `next` для ітератора, створеного з вектора.

<span class="filename">Файл: src/lib.rs</span>

```rust,noplayground
{{#rustdoc_include ../listings/ch13-functional-features/listing-13-12/src/lib.rs:here}}
```


<span class="caption">Блок коду 13-12: виклик методу `next` для ітератора</span>

Зверніть увагу, що нам потрібно зробити `v1_iter` мутабельним: виклик методу `next` для ітератора змінює його внутрішній стан, який використовується для відстеження, де він знаходиться в послідовності. Іншими словами, цей код *поглинає*, чи використовує, ітератор. Кожен виклик `next` з'їдає елемент з ітератора. Нам не треба було робити `v1_iter` мутабельним, коли ми використали його в циклі `for`, бо цикл взяв володіння `v1_iter` і зробив його мутабельним за лаштунками.

Також зверніть увагу, що значення, які ми отримуємо від викликів `next`, є немутабельними посиланнями на значення у векторі. Метод `iter` створює ітератор по незмінних посиланнях. Якщо ми хочемо створити ітератор, який приймає володіння `v1` і повертає значення, що належать нам, ми можемо викликати `into_iter` замість `iter`. Аналогічно, якщо ми хочемо ітерувати по мутабельних посиланнях, ми можемо викликати `iter_mut` замість `iter`.

### Методи, що поглинають ітератор

Трейт `Iterator` має ряд різних методів з реалізаціями по замовчуванню що надаються стандартною бібліотекою; ви можете дізнатися про ці методи в стандартній документації API для трейта `Iterator`. Деякі з цих методів викликають у своєму визначенні метод `next`, чому і необхідно визначити метод `next` при реалізації трейта `Iterator`.

Методи, що викликають `next`, звуться *поглинаючими адапторами*, бо їх виклик використовує ітератор. Один із прикладів - це метод `sum`, який бере володіння ітератором і ітерує по елементах, раз за разом викликаючи `next`, таким чином поглинаючи ітератор. Під час ітерації він додає кожен елемент до поточної загальної суми і повертає загальну суму, коли ітерація завершена. Блок коду 13-13 має тест, що ілюструє використання методу `sum`:

<span class="filename">Файл: src/lib.rs</span>

```rust,noplayground
{{#rustdoc_include ../listings/ch13-functional-features/listing-13-13/src/lib.rs:here}}
```


<span class="caption">Блок коду 13-13: виклик методу `sum` для отримання загальної суми усіх елементів ітератора</span>

Нам не дозволено використовувати `v1_iter` після виклику `sum`, оскільки `sum` перебирає володіння ітератором, на якому його викликано.

### Методи, що створюють інші ітератори

*Адаптери ітераторів* - це методи, визначені для трейта `Iterator`, які не поглинають ітератор. Натомість вони створюють інші ітератори, змінюючи певний аспект оригінального ітератора.

Блок коду 13-17 показує приклад виклику метода-адаптора ітератора `map`, який приймає замикання, яке викличе для кожного елементу під час ітерації. Метод `map` повертає новий ітератор, який виробляє модифіковані елементи. Замикання створює новий ітератор, у якому кожен елемент вектора буде збільшено на 1:

<span class="filename">Файл: src/main.rs</span>

```rust,not_desired_behavior
{{#rustdoc_include ../listings/ch13-functional-features/listing-13-14/src/main.rs:here}}
```


<span class="caption">Блок коду 13-14: Виклик адаптора `map` для створення нового ітератора</span>

Однак, цей код видає попередження:

```console
{{#include ../listings/ch13-functional-features/listing-13-14/output.txt}}
```

Код у Блоці коду 13-14 нічого не робить; замикання, яке ми вказали, ніколи не було викликано. Попередження нагадує нам, чому: адаптори ітераторів ліниві, і нам потрібно поглинути ітератор.

Щоб виправити це попередження і поглинути ітератор, ми використаємо метод `collect`, який ми використовували у Розділі 12 із `env::args` у Блоці коду 12-1. Цей метод поглинає ітератор і збирає отримані в результаті значення в колекцію.

У Блоці коду 13-15 ми зібрали результати ітерування по ітератору, повернутому викликом `map`, у вектор. Цей вектор в результаті міститиме всі елементи оригінального вектора, збільшені на 1.

<span class="filename">Filename: src/main.rs</span>

```rust
{{#rustdoc_include ../listings/ch13-functional-features/listing-13-15/src/main.rs:here}}
```


<span class="caption">Listing 13-15: Calling the `map` method to create a new iterator and then calling the `collect` method to consume the new iterator and create a vector</span>

Because `map` takes a closure, we can specify any operation we want to perform on each item. This is a great example of how closures let you customize some behavior while reusing the iteration behavior that the `Iterator` trait provides.

You can chain multiple calls to iterator adaptors to perform complex actions in a readable way. But because all iterators are lazy, you have to call one of the consuming adaptor methods to get results from calls to iterator adaptors.

### Using Closures that Capture Their Environment

Many iterator adapters take closures as arguments, and commonly the closures we’ll specify as arguments to iterator adapters will be closures that capture their environment. For this example, we’ll use the `filter` method that takes a closure. The closure gets an item from the iterator and returns a Boolean. If the closure returns `true`, the value will be included in the iteration produced by `filter`. If the closure returns `false`, the value won’t be included.

In Listing 13-16, we use `filter` with a closure that captures the `shoe_size` variable from its environment to iterate over a collection of `Shoe` struct instances. It will return only shoes that are the specified size.

<span class="filename">Filename: src/lib.rs</span>

```rust,noplayground
{{#rustdoc_include ../listings/ch13-functional-features/listing-13-16/src/lib.rs}}
```


<span class="caption">Listing 13-16: Using the `filter` method with a closure that captures `shoe_size`</span>

The `shoes_in_size` function takes ownership of a vector of shoes and a shoe size as parameters. It returns a vector containing only shoes of the specified size.

In the body of `shoes_in_size`, we call `into_iter` to create an iterator that takes ownership of the vector. Then we call `filter` to adapt that iterator into a new iterator that only contains elements for which the closure returns `true`.

The closure captures the `shoe_size` parameter from the environment and compares the value with each shoe’s size, keeping only shoes of the size specified. Finally, calling `collect` gathers the values returned by the adapted iterator into a vector that’s returned by the function.

The test shows that when we call `shoes_in_size`, we get back only shoes that have the same size as the value we specified.
