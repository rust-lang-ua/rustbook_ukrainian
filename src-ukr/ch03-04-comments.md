## Коментарі

Всі програмісти прагнуть зробити свій код зрозумілішим, та деколи не завадить додаткове пояснення. В таких випадках програмісти лишають в початковому коді *коментарі*, які ігнорує компілятор, але які можуть бути корисними людям, що читатимуть цей код.

Ось простий коментар:

```rust
// hello, world
```

У Rust найтиповіший стиль коментарів починається з двох знаків дробу і продовжується до кінця рядка. Для коментарів, що займають більше одного рядка, вам доведеться ставити `//` у кожному рядку, ось так:

```rust
// Тут ми робимо щось складне, досить довге, щоб нам знадобилося кілька рядків
// коментаря! Хух! Сподіваюся, цей коментар достатньо детально пояснює, що 
// тут відбувається.
```

Коментарі також можна розміщувати в кінці рядків, що містять код:

<span class="filename">Файл: src/main.rs</span>

```rust
{{#rustdoc_include ../listings/ch03-common-programming-concepts/no-listing-24-comments-end-of-line/src/main.rs}}
```

But you’ll more often see them used in this format, with the comment on a separate line above the code it’s annotating:

<span class="filename">Файл: src/main.rs</span>

```rust
{{#rustdoc_include ../listings/ch03-common-programming-concepts/no-listing-25-comments-above-line/src/main.rs}}
```

Rust also has another kind of comment, documentation comments, which we’ll discuss in the “Publishing a Crate to Crates.io” section of Chapter 14.
