## Написання Повідомлень Про Помилки в Standard Error Замість Стандартного Виводу

Наразі ми записуємо увесь наш вивід в термінал використовуючи макрос `println!`. В більшості терміналів є два типи виводу: *standard output* (`stdout`) для загальної інформації та *standard error* (`stderr`) для повідомлень про помилки. Це розділення дозволяє користувачам направити вдалий вивід програми в файл, але все ще виводити повідомлення про помилки на екрані.

The `println!` macro is only capable of printing to standard output, so we have to use something else to print to standard error.

### Перевірка Де Написані Помилки

Спочатку, нумо побачимо, як контент який `minigrep` виводить в консолі наразі записується в стандартний вивід, включаючи будь-які повідомлення про помилки, які замість цього ми хочемо записувати в Standard Error. Ми зробимо це перенаправивши потік стандартного виводу в файл водночас із навмисним спричиненням помилки. Ми не будемо перенаправляти стандартний помилковій потік, тому будь-який контент відправлений в Standard Error буде продовжувати показуватися на екрані.

Очікується, що програми командного рядка надсилатимуть помилкові повідомлення в потік Standard Error, щоб ми все ще могли бачити помилкові повідомлення на екрані, навіть якщо ми перенаправляємо потік стандартного виводу в файл. Наразі наша програма не добре налаштована: ми побачимо, як вона збереже вивід помилкового повідомлення в файл натомість!

Щоб продемонструвати цю поведінку, ми запустимо програму з `>` і шляхом до файлу, *output.txt*, якому ми хочемо перенаправити потік стандартного виводу. Ми не будемо передавати аргументи, що спричинить помилку:

```console
$ cargo run > output.txt
```

Синтаксис `>` вказує shell писати вміст стандартного виводу в *output.txt* замість екрана. Ми не бачимо помилкове повідомлення, яке ми очікували виведеним на екран, тому це означає, що воно опинилося в файлі. Це те, що містить *output.txt*:

```text
Problem parsing arguments: not enough arguments
```

Так, наше помилкове повідомлення виводиться в консолі стандартного виводу. Це набагато корисніше для таких помилкових повідомлень, щоб вони виводилися в консолі Standard Error та щоб тільки дані від вдалих запусків опинялися в файлі. Ми змінимо це.

### Вивід в Консолі Помилок в Standard Error

Ми використаємо код в Блоці Коду 12-24 для зміни виводу помилкових повідомлень в консолі. Через рефакторінг, який ми робили раніше в цьому розділі, увесь код який виводить помилкові повідомлення перебуває в одній функції, `main`. Стандартна бібліотека надає макрос `eprintln!` який виводить в консолі потоку Standard Error, тому змінимо два місця, де ми викликаємо `println!`, використовуючи замість цього `eprintln!` для виводу в консолі.

<span class="filename">Файл: src/main.rs</span>

```rust,ignore
{{#rustdoc_include ../listings/ch12-an-io-project/listing-12-24/src/main.rs:here}}
```


<span class="caption">Listing 12-24: Writing error messages to standard error instead of standard output using `eprintln!`</span>

Let’s now run the program again in the same way, without any arguments and redirecting standard output with `>`:

```console
$ cargo run > output.txt
Problem parsing arguments: not enough arguments
```

Now we see the error onscreen and *output.txt* contains nothing, which is the behavior we expect of command line programs.

Let’s run the program again with arguments that don’t cause an error but still redirect standard output to a file, like so:

```console
$ cargo run -- to poem.txt > output.txt
```

We won’t see any output to the terminal, and *output.txt* will contain our results:

<span class="filename">Файл: output.txt</span>

```text
Are you nobody, too?
How dreary to be somebody!
```

This demonstrates that we’re now using standard output for successful output and standard error for error output as appropriate.

## Підсумок

В цьому розділі ми пригадали деякі основні концепти, які ви вивчили раніше та розглянули як виконувати базові I/O операції в Rust. Використовуючи аргументи командного рядка, файли, змінні середовища та макрос `eprintln!` для виведення помилок в консолі, тепер ви готові написати застосунок для командного рядка. Поєднавши це з концептами з попередніх розділів, ваш код буде добре організованим, ефективно збирати дані в відповідні структури даних, вдало обробляти помилки та буде добре перевіреним.

Next, we’ll explore some Rust features that were influenced by functional languages: closures and iterators.
