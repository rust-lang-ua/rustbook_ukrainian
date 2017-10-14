## Зрізи

> Від перекладача: приклад у цьому розділі написаний лише для кращого розуміння
> концепції зрізів. Зокрема, спроба замінити рядок на кириличний може призвести
> до неочікуваних наслідків. Причина таких проблем роз'яснуюється у Розділі 8.2.

Інший тип данних, що не тримає власності - *зріз* (*slice*). Зрізи дозволяють 
посилатися на неперервні послідовності елементів в колекції замість усієї 
колекції.

Існує така проста програмістська задача: написати функцію, що приймає стрічку
і повертає перше слово, яке знаходиться в цій стрічці. Якщо функція не знайде 
пробіл у стрічці, це означає що вся стрічка є одним словом і, відтак, функція
має повернути всю стрічку.

Спробуємо написати сигнатуру цієї функції?

```rust,ignore
fn first_word(s: &String) -> ?
```

Ця функція, `first_word`, приймає параметром `&String`. Нам не потрібна 
власність, тому це нормально. Але що ми маємо повернути? У нас немає способу,
що виразити *частину* стрічки. Тим не менш, ми можемо повернути індекс кінця
слова. Спробуємо зробити це у Роздруку 4-10:

<figure>
<span class="filename">Файл: src/main.rs</span>

```rust
fn first_word(s: &String) -> usize {
    let bytes = s.as_bytes();

    for (i, &item) in bytes.iter().enumerate() {
        if item == b' ' {
            return i;
        }
    }

    s.len()
}
```

<figcaption>

Роздрук 4-10: Функція `first_word`, що повертає індекс символа пробіла 
у параметрі типу `String`.

</figcaption>
</figure>

Давайте трохи розберемо цей код. Оскільки нам треба перебрати стрічку у 
параметрі `s` елемент за елементом і перевірити, чи не пробіл це, ми 
перетворюємо стрічку на масив байтів за допомогою методу `as_bytes`:

```rust,ignore
let bytes = s.as_bytes();
```

Далі ми створюємо ітератор по масиву байтів за допомогою методі `iter`:

```rust,ignore
for (i, &item) in bytes.iter().enumerate() {
```

Ітератори будуть детальніше обговорені в Розділі 16. Поки що достатньо знати, 
що `iter` - метод, що повертає кожен елемент в колекції, а метод `enumerate` 
обгортає результат у кортеж, перший елемент якого - індекс, а другий - посилання
на елемент. Це трохи зручніше, ніж обчислювати індекс самостійно.

Оскільки метод `enumerate` повертає кортеж, ми використаємо шаблон для деструктуризації цього кортежу. В циклі `for` ми визначаємо шаблон, що 
складається з індексу `i` і елементу `&item` в кортежі. `&` в шаблоні позначає,
що це посилання.

Ми шукаємо байт, який представляє символ пробілу, за допомогою байтового 
літералу. Коли знаходимо його, повертаємо його індекс. Якщо цього не сталося, 
повертаємо довжину стрічки за допомогою методу `s.len()`:

```rust,ignore
    if item == b' ' {
        return i;
    }
}
s.len()
```

Тепер ми маємо спосіб знайти індекс кінця першого слова у стрічці, але є 
проблема. Ми повертаємо одне значення `usize`, але це значення має сенс лише в 
контексті нашої стрічки. Іншими словами, оскільки це значення не пов'язане із зі 
стрічкою, немає гарантії, що воно буде коректним у подальшому. Розглянемо 
програму у Роздруку 4-11, що використовує функцію `first_word` з Роздруку 4-10:

<figure>
<span class="filename">Файл: src/main.rs</span>

```rust
# fn first_word(s: &String) -> usize {
#     let chars = s.chars();
#
#     for (i, item) in chars.enumerate() {
#         if item == ' ' {
#             return i;
#         }
#     }
#
#     s.len()
# }
#
fn main() {
    let mut s = String::from("hello world");

    let word = first_word(&s); // word матиме значення 5

    s.clear(); // Очищуємо s, так що його значення стає "".

    // word все ще містить значення 5, але рядка, в якому можна використати
    // це значення, вже не існує. word має некоректне значення!
}
```

<figcaption>

Роздрук 4-11: Збереження результату виклику функції `first_word` і наступна 
зміна вмісту стрічки

</figcaption>
</figure>

Ця програма компілюється без помилок, і також скопмілювалася б, якби ви 
використали `word` після виклику `s.clear()`. `word` ніяк не пов'язане зі станом
`s`, і тому `word` міститиме значення `5`. Ми можемо використати це значення `5` зі змінною `s`, щоб спробувати видобути з неї перше слово, але це буде помилкою,
бо вміст `s` змінився відколи ми зберегли `5` до `word`.

Необхідність дбати про актуальність індексу в `word` відносно даних в `s` нудно 
і може спровокувати помилки! Керування такими індексами стає ще більш ламким,
якщо ми напишемо функцію `second_word`. Її сигнатура буде виглядати так:

```rust,ignore
fn second_word(s: &String) -> (usize, usize) {
```

Тепер ми відстежуємо початковий *і* кінцевий індекси, і ми маємо ще більше 
значень, обчислених з даних у конкретному стані, але ніяк не прив'язаних до 
цього стану. Тепер ми маємо три непов'язані змінні, підвішені в повітрі, які нам
треба тримати синхронізованими.

На щастя, у Rust є рішення цієї проблеми: зрізи стрічок.

### Зрізи стрічок

*Зріз стрічки* - це посилання на частину стрічки `String`, і виглядає воно так:

```rust
let s = String::from("hello world");

let hello = &s[0..5];
let world = &s[6..11];
```

Це схоже на посилання на всю стрічку `String`, але з додатковим шматком 
`[0..5]`. Замість того, щоб посилатися на всю `String`, воно посилається на 
внутрішню частину в `String` і число елементів, яких воно стосується.

Ми створюємо зрізи з межами `[початковий_індекс..кінцевий_індекс]`, але 
структура даних зрізу насправді зберігає початкову позицію і довжину зрізу. Тому
у випадку `let world = &s[6..11];`, `world` буде зрізом, що складається зі вказівника на 6-й байт `s` і довжини 5.

Рисунок 4-12 показує це як діаграму.

<figure>
<img alt="world містить вказівник на 6-й байт стрічки s і довжину 5" src="img/trpl04-06.svg" class="center" style="width: 50%;" />

<figcaption>

Рисунок 4-12: зріз стрічки, що посилається на частину `String`.

</figcaption>
</figure>

Синтаксис меж `..` у Rust дозволяє, якщо ви хочете почати зріз на початковому
індексі (нуль), пропустити значення перед крапками. Іншими словами, ці рядки
тотожні:

```rust
let s = String::from("hello");

let slice = &s[0..2];
let slice = &s[..2];
```

Так само якщо ваш зріз включає останній байт стрічки, ви можете пропустити 
останнє число. Таким чином, ці рядки також тотжні:

```rust
let s = String::from("hello");

let len = s.len();

let slice = &s[3..len];
let slice = &s[3..];
```

Також можна пропустити обидва значення, щоб взяти зріз з усієї стрічки. Це також
тотожні рядки:

```rust
let s = String::from("hello");

let len = s.len();

let slice = &s[0..len];
let slice = &s[..];
```

Озброєні цими знаннями, перепишемо `first_word`, щоб вона повертала зріз. Тип, 
що позначає зріз стрічки, записується як `&str`:

<span class="filename">Файл: src/main.rs</span>

```rust
fn first_word(s: &String) -> &str {
    let bytes = s.as_bytes();

    for (i, &item) in bytes.iter().enumerate() {
        if item == b' ' {
            return &s[0..i];
        }
    }

    &s[..]
}
```

Ми отримуємо індекс кінця слова тим же чином, що й у Роздруку 4-10, пошуком
першого стрічного пробілу. Коли ми знаходимо пробіл, ми повертаємо зріз стрічки
за допомогою початку стрічки і індексу знайденого пробілу як початкового і кінцевого індексів.

Now when we call `first_word`, we get back a single value that is tied to the
underlying data. The value is made up of a reference to the starting point of
the slice and the number of elements in the slice.

Returning a slice would also work for a `second_word` function:

```rust,ignore
fn second_word(s: &String) -> &str {
```

We now have a straightforward API that’s much harder to mess up, since the
compiler will ensure the references into the `String` remain valid. Remember
the bug in the program in Listing 4-11, when we got the index to the end of the
first word but then cleared the string so our index was invalid? That code was
logically incorrect but didn’t show any immediate errors. The problems would
show up later if we kept trying to use the first word index with an emptied
string. Slices make this bug impossible and let us know we have a problem with
our code much sooner. Using the slice version of `first_word` will throw a
compile time error:

<span class="filename">Filename: src/main.rs</span>

```rust,ignore
fn main() {
    let mut s = String::from("hello world");

    let word = first_word(&s);

    s.clear(); // Error!
}
```

Here’s the compiler error:

```text
17:6 error: cannot borrow `s` as mutable because it is also borrowed as
            immutable [E0502]
    s.clear(); // Error!
    ^
15:29 note: previous borrow of `s` occurs here; the immutable borrow prevents
            subsequent moves or mutable borrows of `s` until the borrow ends
    let word = first_word(&s);
                           ^
18:2 note: previous borrow ends here
fn main() {

}
^
```

Recall from the borrowing rules that if we have an immutable reference to
something, we cannot also take a mutable reference. Because `clear` needs to
truncate the `String`, it tries to take a mutable reference, which fails. Not
only has Rust made our API easier to use, but it has also eliminated an entire
class of errors at compile time!

#### String Literals Are Slices

Recall that we talked about string literals being stored inside the binary. Now
that we know about slices, we can properly understand string literals:

```rust
let s = "Hello, world!";
```

The type of `s` here is `&str`: it’s a slice pointing to that specific point of
the binary. This is also why string literals are immutable; `&str` is an
immutable reference.

#### String Slices as Parameters

Knowing that you can take slices of literals and `String`s leads us to one more
improvement on `first_word`, and that’s its signature:

```rust,ignore
fn first_word(s: &String) -> &str {
```

A more experienced Rustacean would write the following line instead because it
allows us to use the same function on both `String`s and `&str`s:

```rust,ignore
fn first_word(s: &str) -> &str {
```

If we have a string slice, we can pass that directly. If we have a `String`, we
can pass a slice of the entire `String`. Defining a function to take a string
slice instead of a reference to a String makes our API more general and useful
without losing any functionality:

<span class="filename">Filename: src/main.rs</span>

```rust
# fn first_word(s: &str) -> &str {
#     let bytes = s.as_bytes();
#
#     for (i, &item) in bytes.iter().enumerate() {
#         if item == b' ' {
#             return &s[0..i];
#         }
#     }
#
#     &s[..]
# }
fn main() {
    let my_string = String::from("hello world");

    // first_word works on slices of `String`s
    let word = first_word(&my_string[..]);

    let my_string_literal = "hello world";

    // first_word works on slices of string literals
    let word = first_word(&my_string_literal[..]);

    // since string literals *are* string slices already,
    // this works too, without the slice syntax!
    let word = first_word(my_string_literal);
}
```

### Other Slices

String slices, as you might imagine, are specific to strings. But there’s a
more general slice type, too. Consider this array:

```rust
let a = [1, 2, 3, 4, 5];
```

Just like we might want to refer to a part of a string, we might want to refer
to part of an array and would do so like this:

```rust
let a = [1, 2, 3, 4, 5];

let slice = &a[1..3];
```

This slice has the type `&[i32]`. It works the same way as string slices do, by
storing a reference to the first element and a length. You’ll use this kind of
slice for all sorts of other collections. We’ll discuss these collections in
detail when we talk about vectors in Chapter 8.

## Summary

The concepts of ownership, borrowing, and slices are what ensure memory safety
in Rust programs at compile time. The Rust language gives you control over your
memory usage like other systems programming languages, but having the owner of
data automatically clean up that data when the owner goes out of scope means
you don’t have to write and debug extra code to get this control.

Ownership affects how lots of other parts of Rust work, so we’ll talk about
these concepts further throughout the rest of the book. Let’s move on to the
next chapter and look at grouping pieces of data together in a `struct`.
