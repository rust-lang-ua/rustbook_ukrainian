## Зберігання списків значень у векторах

Перший тип колекцій, який ми розглянемо - це `Vec<T>`, також відомий як *вектор*. Вектори дозволять вам зберігати більше одного значення в єдиній структурі даних, що розташовує ці значення поруч один з одним у пам'яті. Вектор може зберігати лише значення одного типу. Вони корисні, коли ви маєте список предметів, наприклад рядки тексту у файлі або ціни на товари у кошику.

### Створення нового вектора

Щоб створити новий порожній вектор, ми викликаємо `Vec:new`, як показано в Блоці коду 8-1.

```rust
{{#rustdoc_include ../listings/ch08-common-collections/listing-08-01/src/main.rs:here}}
```


<span class="caption">Блок коду 8-1: створення нового порожнього вектора для зберігання значень типу `i32`</span>

Зауважте, що тут ми додали анотації типу. Оскільки ми не вставляємо жодного значення в цей вектор, Rust не знає, які елементи ми маємо намір зберігати. Це важлива деталь. Вектори реалізовані за допомогою узагальнень; ми розкажемо, як використовувати узагальнення з вашими власними типами в Розділі 10. Наразі треба знати лише, що тип `Vec<T>`, наданий стандартною бібліотекою, може містити будь-який тип. Коли ми створюємо вектор, що міститиме певний тип, ми можемо зазначити тип у кутових дужках. У Блоці коду 8-1 ми кажемо Rust, що `Vec<T>` у `v` міститиме елементи типу `i32`.

Зазвичай ви створюватимете `Vec<T>` з початковими значеннями, і Rust виведе тип значень, які ви хочете зберігати, тож вам нечасто буде потрібно додавати таку анотацію типу. Rust для зручності надає макрос `vec!`, який створює новий вектор, який містить ваші значення. Блок коду 8-2 створює новий `Vec<i32>`, що містить значення `1`, `2`, і `3`. Тип цілих - `i32`, бо це тип цілих за замовчуванням, як ми вже говорили в підрозділі [“Типи даних”][data-types]<!-- ignore --> Розділу 3.

```rust
{{#rustdoc_include ../listings/ch08-common-collections/listing-08-02/src/main.rs:here}}
```


<span class="caption">Блок коду 8-2: створення нового вектора, що містить значення</span>

Оскільки ми надали початкові значення `i32`, Rust може вивести, що типом `v` є `Vec<i32>` і анотація типу тут не потрібна. Далі ми поглянемо, як змінити вектор.

### Оновлення вектора

Щоб створити вектор і додати до нього елементи ми можемо використати метод `push`, як показано в Блоці коду 8-3.

```rust
{{#rustdoc_include ../listings/ch08-common-collections/listing-08-03/src/main.rs:here}}
```


<span class="caption">Блок коду 8-3: використання методу `push` для додавання значень у вектор</span>

Як і для будь-якої змінної, якщо ми хочемо змінювати її значення, ми повинні зробити його мутабельним за допомогою ключового слова `mut`, як говорилося в Розділі 3. Числа, як ми розміщуємо у векторі, мають тип `i32`, і Rust виводить це з даних, тож нам не потрібна анотація `Vec<i32>`.

### Читання елементів векторів

Існує два способи послатися на значення, що зберігається у векторі: через індексацію або використовуючи метод `get`. У наступних прикладах ми анотували типи значень, які повертаються з цих функцій, для додаткової ясності.

Блок коду 8-4 показує обидва методи доступу до значення у векторі - синтаксис індексування і метод `get`.

```rust
{{#rustdoc_include ../listings/ch08-common-collections/listing-08-04/src/main.rs:here}}
```


<span class="caption">Блок коду 8-4: використання синтаксису індексів або методу `get` для доступу до елементів вектора</span>

Note a few details here. We use the index value of `2` to get the third element because vectors are indexed by number, starting at zero. Using `&` and `[]` gives us a reference to the element at the index value. When we use the `get` method with the index passed as an argument, we get an `Option<&T>` that we can use with `match`.

The reason Rust provides these two ways to reference an element is so you can choose how the program behaves when you try to use an index value outside the range of existing elements. As an example, let’s see what happens when we have a vector of five elements and then we try to access an element at index 100 with each technique, as shown in Listing 8-5.

```rust,should_panic,panics
{{#rustdoc_include ../listings/ch08-common-collections/listing-08-05/src/main.rs:here}}
```


<span class="caption">Listing 8-5: Attempting to access the element at index 100 in a vector containing five elements</span>

When we run this code, the first `[]` method will cause the program to panic because it references a nonexistent element. This method is best used when you want your program to crash if there’s an attempt to access an element past the end of the vector.

When the `get` method is passed an index that is outside the vector, it returns `None` without panicking. You would use this method if accessing an element beyond the range of the vector may happen occasionally under normal circumstances. Your code will then have logic to handle having either `Some(&element)` or `None`, as discussed in Chapter 6. For example, the index could be coming from a person entering a number. If they accidentally enter a number that’s too large and the program gets a `None` value, you could tell the user how many items are in the current vector and give them another chance to enter a valid value. That would be more user-friendly than crashing the program due to a typo!

When the program has a valid reference, the borrow checker enforces the ownership and borrowing rules (covered in Chapter 4) to ensure this reference and any other references to the contents of the vector remain valid. Recall the rule that states you can’t have mutable and immutable references in the same scope. That rule applies in Listing 8-6, where we hold an immutable reference to the first element in a vector and try to add an element to the end. This program won’t work if we also try to refer to that element later in the function:


```rust,ignore,does_not_compile
{{#rustdoc_include ../listings/ch08-common-collections/listing-08-06/src/main.rs:here}}
```


<span class="caption">Listing 8-6: Attempting to add an element to a vector while holding a reference to an item</span>

Compiling this code will result in this error:


```console
{{#include ../listings/ch08-common-collections/listing-08-06/output.txt}}
```

The code in Listing 8-6 might look like it should work: why should a reference to the first element care about changes at the end of the vector? This error is due to the way vectors work: because vectors put the values next to each other in memory, adding a new element onto the end of the vector might require allocating new memory and copying the old elements to the new space, if there isn’t enough room to put all the elements next to each other where the vector is currently stored. In that case, the reference to the first element would be pointing to deallocated memory. The borrowing rules prevent programs from ending up in that situation.

> Note: For more on the implementation details of the `Vec<T>` type, see [“The Rustonomicon”][nomicon].

### Iterating over the Values in a Vector

To access each element in a vector in turn, we would iterate through all of the elements rather than use indices to access one at a time. Listing 8-7 shows how to use a `for` loop to get immutable references to each element in a vector of `i32` values and print them.

```rust
{{#rustdoc_include ../listings/ch08-common-collections/listing-08-07/src/main.rs:here}}
```


<span class="caption">Listing 8-7: Printing each element in a vector by iterating over the elements using a `for` loop</span>

We can also iterate over mutable references to each element in a mutable vector in order to make changes to all the elements. The `for` loop in Listing 8-8 will add `50` to each element.

```rust
{{#rustdoc_include ../listings/ch08-common-collections/listing-08-08/src/main.rs:here}}
```


<span class="caption">Listing 8-8: Iterating over mutable references to elements in a vector</span>

To change the value that the mutable reference refers to, we have to use the `*` dereference operator to get to the value in `i` before we can use the `+=` operator. We’ll talk more about the dereference operator in the [“Following the Pointer to the Value with the Dereference Operator”][deref]<!-- ignore -->
section of Chapter 15.

Iterating over a vector, whether immutably or mutably, is safe because of the borrow checker's rules. If we attempted to insert or remove items in the `for` loop bodies in Listing 8-7 and Listing 8-8, we would get a compiler error similar to the one we got with the code in Listing 8-6. The reference to the vector that the `for` loop holds prevents simultaneous modification of the whole vector.

### Використання енума для зберігання декількох типів

Вектор може зберігати лише значення одного типу. Це може бути незручно; точно існують випадки, коли є потреба у зберіганні списку елементів різних типів. На щастя, варіанти енума визначені як один тип, тож коли нам потрібен один тип для представлення елементів різних типів, ми можемо визначити і використовувати енум!

Наприклад, нехай ми хочемо отримати значення з рядка в таблиці, у якій деякі стовпці в рядку містять цілі числа, деякі — числа з рухомими точками, а деякі — рядки. Ми можемо визначити енум, варіанти якого будуть містити різні типи значень, і всі варіанти енума будуть вважатися одним і тим же типом — енумом. Тоді ми можемо створити вектор, який міститиме цей енум і, зрештою, міститиме різні типи. Ми продемонстрували це у Блоці коду 8-9.

```rust
{{#rustdoc_include ../listings/ch08-common-collections/listing-08-09/src/main.rs:here}}
```


<span class="caption">Блок коду 8-9: Визначення `enum` для зберігання значень різних типів у одному векторі</span>

Rust має знати, які типи будуть у векторі, під час компіляції, щоб знати, скільки саме пам'яті у купі буде потрібно для зберігання кожного елемента. Ми також маємо явно зазначити, які типи можуть бути в цьому векторі. Якби Rust дозволив вектору містити будь-який тип, була б імовірність, що один або кілька з типів призведуть до помилок при виконанні операцій на елементах вектора. Використання енуму і виразу `match` означає, що Rust гарантує під час компіляції, що умі можливі випадки буде оброблено, як обговорювалося в Розділі 6.

Якщо ви не маєте вичерпного списку типів, з якими програма працюватиме під час виконання для зберігання у векторі, техніка енумів не спрацює. Натомість ви можете скористатися трейтовими об'єктами, про які йдеться у Розділі 17.

Now that we’ve discussed some of the most common ways to use vectors, be sure to review [the API documentation][vec-api]<!-- ignore --> for all the many useful methods defined on `Vec<T>` by the standard library. For example, in addition to `push`, a `pop` method removes and returns the last element.

### Dropping a Vector Drops Its Elements

Like any other `struct`, a vector is freed when it goes out of scope, as annotated in Listing 8-10.

```rust
{{#rustdoc_include ../listings/ch08-common-collections/listing-08-10/src/main.rs:here}}
```


<span class="caption">Listing 8-10: Showing where the vector and its elements are dropped</span>

When the vector gets dropped, all of its contents are also dropped, meaning the integers it holds will be cleaned up. The borrow checker ensures that any references to contents of a vector are only used while the vector itself is valid.

Let’s move on to the next collection type: `String`!

[data-types]: ch03-02-data-types.html#data-types
[nomicon]: ../nomicon/vec/vec.html
[vec-api]: ../std/vec/struct.Vec.html
[deref]: ch15-02-deref.html#following-the-pointer-to-the-value-with-the-dereference-operator
