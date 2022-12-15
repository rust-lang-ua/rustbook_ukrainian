# Мова програмування Rust

[The Rust Programming Language](title-page.md) [Foreword](foreword.md) [Introduction](ch00-00-introduction.md)

## Розпочинаємо

- [Початок](ch01-00-getting-started.md)
    - [Встановлення](ch01-01-installation.md)
    - [Hello, World!](ch01-02-hello-world.md)
    - [Hello, Cargo!](ch01-03-hello-cargo.md)

- [Програмування гри - відгадайки](ch02-00-guessing-game-tutorial.md)

- [Загальні концепції програмування](ch03-00-common-programming-concepts.md)
    - [Змінні та мутабельність](ch03-01-variables-and-mutability.md)
    - [Типи даних](ch03-02-data-types.md)
    - [Функції](ch03-03-how-functions-work.md)
    - [Коментарі](ch03-04-comments.md)
    - [Управління потоком виконання](ch03-05-control-flow.md)

- [Розуміння володіння](ch04-00-understanding-ownership.md)
    - [Що таке володіння?](ch04-01-what-is-ownership.md)
    - [Посилання і позичання](ch04-02-references-and-borrowing.md)
    - [Тип даних слайс](ch04-03-slices.md)

- [Використання структур для структурування пов'язаних даних](ch05-00-structs.md)
    - [Визначення і інстанціювання структур Struct](ch05-01-defining-structs.md)
    - [Приклад програми, що використовує структури](ch05-02-example-structs.md)
    - [Синтаксис методів](ch05-03-method-syntax.md)

- [Енуми і зіставлення з шаблоном](ch06-00-enums.md)
    - [Визначення енума](ch06-01-defining-an-enum.md)
    - [Конструкція управління `match`](ch06-02-match.md)
    - [Лаконічний контроль виконання конструкцією `if let`](ch06-03-if-let.md)

## Базова грамотність Rust

- [Керування проєктами, що зростають, за допомогою пакетів, крейтів та модулів](ch07-00-managing-growing-projects-with-packages-crates-and-modules.md)
    - [Пакети та крейти](ch07-01-packages-and-crates.md)
    - [Визначення модулів для управління областями видимості та приватністю](ch07-02-defining-modules-to-control-scope-and-privacy.md)
    - [Referring to Names in Different Modules](ch07-03-paths-for-referring-to-an-item-in-the-module-tree.md)
    - [Підключення шляхів до області видимості за допомогою ключового слова `use`](ch07-04-bringing-paths-into-scope-with-the-use-keyword.md)
    - [Розподіл модулів на різні файли](ch07-05-separating-modules-into-different-files.md)

- [Common Collections](ch08-00-common-collections.md)
    - [Vectors](ch08-01-vectors.md)
    - [Strings](ch08-02-strings.md)
    - [Hash Maps](ch08-03-hash-maps.md)

- [Обробка помилок](ch09-00-error-handling.md)
    - [Невідновні Помилки із `panic!`](ch09-01-unrecoverable-errors-with-panic.md)
    - [Помилки, що піддаються відновленню за допомогою `Result`](ch09-02-recoverable-errors-with-result.md)
    - [`panic!` чи не `panic!`](ch09-03-to-panic-or-not-to-panic.md)

- [Узагальнені типи, трейти та лайфтайми](ch10-00-generics.md)
    - [Узагальнені типи даних](ch10-01-syntax.md)
    - [Трейти: визначення загальної поведінки](ch10-02-traits.md)
    - [Перевірка коректності посилань за допомогою часів існування](ch10-03-lifetime-syntax.md)

- [Написання автоматизованих тестів](ch11-00-testing.md)
    - [Як писати тести](ch11-01-writing-tests.md)
    - [Контроль над запуском тестів](ch11-02-running-tests.md)
    - [Організація тестів](ch11-03-test-organization.md)

- [Проєкт з введенням/виведенням: створення програми командного рядка](ch12-00-an-io-project.md)
    - [Приймання аргументів командного рядка](ch12-01-accepting-command-line-arguments.md)
    - [Читання файлу](ch12-02-reading-a-file.md)
    - [Рефакторизація для покращення модульності та обробки помилок](ch12-03-improving-error-handling-and-modularity.md)
    - [Розробка Функціонала Бібліотеки із Test-Driven Development](ch12-04-testing-the-librarys-functionality.md)
    - [Робота зі Змінними Середовища](ch12-05-working-with-environment-variables.md)
    - [Написання Повідомлень Про Помилки в Standard Error Замість Стандартного Виводу](ch12-06-writing-to-stderr-instead-of-stdout.md)

## Thinking in Rust

- [Функціональні можливості мови: Ітератори та Замикання](ch13-00-functional-features.md)
    - [Замикання: Анонімні Функції що Захоплюють Своє Середовище](ch13-01-closures.md)
    - [Обробка послідовностей елементів за допомогою ітераторів](ch13-02-iterators.md)
    - [Покращуємо наш проєкт з введенням/виведенням](ch13-03-improving-our-io-project.md)
    - [Порівняння швидкодії: цикли проти ітераторів](ch13-04-performance.md)

- [Більше про Cargo та Crates.io](ch14-00-more-about-cargo.md)
    - [Налаштування Збірок з Release Профілями](ch14-01-release-profiles.md)
    - [Публікація Крейта на Crates.io](ch14-02-publishing-to-crates-io.md)
    - [Робочі Області Cargo](ch14-03-cargo-workspaces.md)
    - [Installing Binaries from Crates.io with `cargo install`](ch14-04-installing-binaries.md)
    - [Розширення Cargo із Користувацькими Командами](ch14-05-extending-cargo.md)

- [Розумні вказівники](ch15-00-smart-pointers.md)
    - [Використання `Box<T>` для вказування на значення в Heap](ch15-01-box.md)
    - [Використання розумних вказівників як звичайних посилань за допомогою трейта `Deref`](ch15-02-deref.md)
    - [Виконання коду при очищенні за допомогою трейту `Drop`](ch15-03-drop.md)
    - [`Rc<T>`, розумний вказівник з лічильником посилань](ch15-04-rc.md)
    - [`RefCell<T>` і шаблон внутрішньої мутабельності](ch15-05-interior-mutability.md)
    - [Цикли посилань можуть призвести до витоку пам'яті](ch15-06-reference-cycles.md)

- [Конкурентність без страху](ch16-00-concurrency.md)
    - [Threads](ch16-01-threads.md)
    - [Message Passing](ch16-02-message-passing.md)
    - [Shared State](ch16-03-shared-state.md)
    - [Extensible Concurrency: `Sync` and `Send`](ch16-04-extensible-concurrency-sync-and-send.md)

- [Is Rust an Object-Oriented Programming Language?](ch17-00-oop.md)
    - [What Does Object-Oriented Mean?](ch17-01-what-is-oo.md)
    - [Trait Objects for Using Values of Different Types](ch17-02-trait-objects.md)
    - [Object-Oriented Design Pattern Implementations](ch17-03-oo-design-patterns.md)

## Advanced Topics

- [Шаблони та Зіставлення Шаблонів](ch18-00-patterns.md)
    - [All the Places Patterns May be Used](ch18-01-all-the-places-for-patterns.md)
    - [Спростовуваність: Чи Може Шаблон Бути Невідповідним](ch18-02-refutability.md)
    - [All the Pattern Syntax](ch18-03-pattern-syntax.md)

- [Просунутий функціонал](ch19-00-advanced-features.md)
    - [Небезпечний Rust](ch19-01-unsafe-rust.md)
    - [Поглиблено про трейти](ch19-03-advanced-traits.md)
    - [Поглиблено про типи](ch19-04-advanced-types.md)
    - [Поглиблено про функції та замикання](ch19-05-advanced-functions-and-closures.md)
    - [Advanced Functions & Closures](ch19-06-macros.md)

- [Останній проєкт: збірка багатопотокового вебсервера](ch20-00-final-project-a-web-server.md)
    - [Збірка однопотокового вебсервера](ch20-01-single-threaded.md)
    - [Перетворюємо наш однопотоковий сервер на багатопотоковий](ch20-02-multithreaded.md)
    - [Плавне вимикання і очищення](ch20-03-graceful-shutdown-and-cleanup.md)

- [Додатки](appendix-00.md)
    - [A - Ключові слова](appendix-01-keywords.md)
    - [B - оператори та символи](appendix-02-operators.md)
    - [C - Derivable Traits](appendix-03-derivable-traits.md)
    - [D - Macros](appendix-04-useful-development-tools.md)
    - [E - Translations](appendix-05-editions.md)
    - [F - Newest Features](appendix-06-translation.md)
    - [Додаток G - як робиться Rust і "щонічний Rust"](appendix-07-nightly-rust.md)
