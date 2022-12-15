## Читання файлу

Тепер додамо функціональність для читання файлу, заданого параметром `file_path`. Для початку нам знадобиться файл для тестування, і ми скористаємося файлом із невеликим текстом у кілька рядків із повторенням слів. Блок коду 12-3 містить вірш Емілі Дікінсон, що добре підійде для цього! Створіть файл *poem.txt* у кореневому рівні вашого проєкту, і введіть вірш "Я ніхто! А ти хто?"

<span class="filename">Файл: poem.txt</span>

```text
{{#include ../listings/ch12-an-io-project/listing-12-03/poem.txt}}
```


<span class="caption">Listing 12-3: A poem by Emily Dickinson makes a good test case</span>

With the text in place, edit *src/main.rs* and add code to read the file, as shown in Listing 12-4.

<span class="filename">Filename: src/main.rs</span>

```rust,should_panic,noplayground
{{#rustdoc_include ../listings/ch12-an-io-project/listing-12-04/src/main.rs:here}}
```


<span class="caption">Listing 12-4: Reading the contents of the file specified by the second argument</span>

First, we bring in a relevant part of the standard library with a `use` statement: we need `std::fs` to handle files.

In `main`, the new statement `fs::read_to_string` takes the `file_path`, opens that file, and returns a `std::io::Result<String>` of the file’s contents.

After that, we again add a temporary `println!` statement that prints the value of `contents` after the file is read, so we can check that the program is working so far.

Let’s run this code with any string as the first command line argument (because we haven’t implemented the searching part yet) and the *poem.txt* file as the second argument:

```console
{{#rustdoc_include ../listings/ch12-an-io-project/listing-12-04/output.txt}}
```

Чудово! Код прочитав і надрукував вміст файлу. Але код має кілька недоліків. Наразі функція `main` відповідає за багато різних речей. В цілому, функції стають зрозумілішими і їх легше підтримувати, якщо кожна функція відповідає за лише одну ідею. Інша проблема полягає в тому, що ми не обробляємо помилки так добре, як могли б. Програма все ще невелика, тому ці недоліки не становлять великої проблеми, але зі зростанням програми стане важчим їх акуратно виправити. Є гарна порада - починати рефакторити код на ранній стадії розробки програми, бо значно легше рефакторити невеликі фрагменти коду. Цим ми й займемося.
