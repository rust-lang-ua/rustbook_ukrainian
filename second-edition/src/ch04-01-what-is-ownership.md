## Що таке володіння?

Основна особливість Rust - це *володіння*. Хоча її досить легко пояснити, вона
має глибокі наслідки для решти мови.

Всі програми мають управляти своїм використанням пам'яті комп'ютера під час 
роботи. Деякі мови мають збирача сміття, який постійно шукає пам'ять, що її вже
не використовують, під час роботи програми; в інших мовах, програміст має явно
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


### Memory and Allocation

In the case of a string literal, we know the contents at compile time so the
text is hardcoded directly into the final executable, making string literals
fast and efficient. But these properties only come from its immutability.
Unfortunately, we can’t put a blob of memory into the binary for each piece of
text whose size is unknown at compile time and whose size might change while
running the program.

With the `String` type, in order to support a mutable, growable piece of text,
we need to allocate an amount of memory on the heap, unknown at compile time,
to hold the contents. This means:

1. The memory must be requested from the operating system at runtime.
2. We need a way of returning this memory to the operating system when we’re
done with our `String`.

That first part is done by us: when we call `String::from`, its implementation
requests the memory it needs. This is pretty much universal in programming
languages.

However, the second part is different. In languages with a *garbage collector
(GC)*, the GC keeps track and cleans up memory that isn’t being used anymore,
and we, as the programmer, don’t need to think about it. Without a GC, it’s the
programmer’s responsibility to identify when memory is no longer being used and
call code to explicitly return it, just as we did to request it. Doing this
correctly has historically been a difficult programming problem. If we forget,
we’ll waste memory. If we do it too early, we’ll have an invalid variable. If
we do it twice, that’s a bug too. We need to pair exactly one `allocate` with
exactly one `free`.

Rust takes a different path: the memory is automatically returned once the
variable that owns it goes out of scope. Here’s a version of our scope example
from Listing 4-1 using a `String` instead of a string literal:

```rust
{
    let s = String::from("hello"); // s is valid from this point forward

    // do stuff with s
}                                  // this scope is now over, and s is no
                                   // longer valid
```

There is a natural point at which we can return the memory our `String` needs
to the operating system: when `s` goes out of scope. When a variable goes out
of scope, Rust calls a special function for us. This function is called `drop`,
and it’s where the author of `String` can put the code to return the memory.
Rust calls `drop` automatically at the closing `}`.

> Note: In C++, this pattern of deallocating resources at the end of an item's
> lifetime is sometimes called *Resource Acquisition Is Initialization (RAII)*.
> The `drop` function in Rust will be familiar to you if you’ve used RAII
> patterns.

This pattern has a profound impact on the way Rust code is written. It may seem
simple right now, but the behavior of code can be unexpected in more
complicated situations when we want to have multiple variables use the data
we’ve allocated on the heap. Let’s explore some of those situations now.

#### Ways Variables and Data Interact: Move

Multiple variables can interact with the same data in different ways in Rust.
Let’s look at an example using an integer in Listing 4-2:

<figure>

```rust
let x = 5;
let y = x;
```

<figcaption>

Listing 4-2: Assigning the integer value of variable `x` to `y`

</figcaption>
</figure>

We can probably guess what this is doing based on our experience with other
languages: “Bind the value `5` to `x`; then make a copy of the value in `x` and
bind it to `y`.” We now have two variables, `x` and `y`, and both equal `5`.
This is indeed what is happening because integers are simple values with a
known, fixed size, and these two `5` values are pushed onto the stack.

Now let’s look at the `String` version:

```rust
let s1 = String::from("hello");
let s2 = s1;
```

This looks very similar to the previous code, so we might assume that the way
it works would be the same: that is, the second line would make a copy of the
value in `s1` and bind it to `s2`. But this isn’t quite what happens.

To explain this more thoroughly, let’s look at what `String` looks like under
the covers in Figure 4-3. A `String` is made up of three parts, shown on the
left: a pointer to the memory that holds the contents of the string, a length,
and a capacity. This group of data is stored on the stack. On the right is the
memory on the heap that holds the contents.

<figure>
<img alt="String in memory" src="img/trpl04-01.svg" class="center" style="width: 50%;" />

<figcaption>

Figure 4-3: Representation in memory of a `String` holding the value `"hello"`
bound to `s1`

</figcaption>
</figure>

The length is how much memory, in bytes, the contents of the `String` is
currently using. The capacity is the total amount of memory, in bytes, that the
`String` has received from the operating system. The difference between length
and capacity matters, but not in this context, so for now, it’s fine to ignore
the capacity.

When we assign `s1` to `s2`, the `String` data is copied, meaning we copy the
pointer, the length, and the capacity that are on the stack. We do not copy the
data on the heap that the pointer refers to. In other words, the data
representation in memory looks like Figure 4-4.

<figure>
<img alt="s1 and s2 pointing to the same value" src="img/trpl04-02.svg" class="center" style="width: 50%;" />

<figcaption>

Figure 4-4: Representation in memory of the variable `s2` that has a copy of
the pointer, length, and capacity of `s1`

</figcaption>
</figure>

The representation does *not* look like Figure 4-5, which is what memory would
look like if Rust instead copied the heap data as well. If Rust did this, the
operation `s2 = s1` could potentially be very expensive in terms of runtime
performance if the data on the heap was large.

<figure>
<img alt="s1 and s2 to two places" src="img/trpl04-03.svg" class="center" style="width: 50%;" />

<figcaption>

Figure 4-5: Another possibility of what `s2 = s1` might do if Rust copied the
heap data as well

</figcaption>
</figure>

Earlier, we said that when a variable goes out of scope, Rust automatically
calls the `drop` function and cleans up the heap memory for that variable. But
Figure 4-4 shows both data pointers pointing to the same location. This is a
problem: when `s2` and `s1` go out of scope, they will both try to free the
same memory. This is known as a *double free* error and is one of the memory
safety bugs we mentioned previously. Freeing memory twice can lead to memory
corruption, which can potentially lead to security vulnerabilities.

To ensure memory safety, there’s one more detail to what happens in this
situation in Rust. Instead of trying to copy the allocated memory, Rust
considers `s1` to no longer be valid and therefore, Rust doesn’t need to free
anything when `s1` goes out of scope. Check out what happens when you try to
use `s1` after `s2` is created:

```rust,ignore
let s1 = String::from("hello");
let s2 = s1;

println!("{}", s1);
```

You’ll get an error like this because Rust prevents you from using the
invalidated reference:

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

If you’ve heard the terms “shallow copy” and “deep copy” while working with
other languages, the concept of copying the pointer, length, and capacity
without copying the data probably sounds like a shallow copy. But because Rust
also invalidates the first variable, instead of calling this a shallow copy,
it’s known as a *move*. Here we would read this by saying that `s1` was *moved*
into `s2`. So what actually happens is shown in Figure 4-6.

<figure>
<img alt="s1 moved to s2" src="img/trpl04-04.svg" class="center" style="width: 50%;" />

<figcaption>

Figure 4-6: Representation in memory after `s1` has been invalidated

</figcaption>
</figure>

That solves our problem! With only `s2` valid, when it goes out of scope, it
alone will free the memory, and we’re done.

In addition, there’s a design choice that’s implied by this: Rust will never
automatically create “deep” copies of your data. Therefore, any *automatic*
copying can be assumed to be inexpensive in terms of runtime performance.

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
