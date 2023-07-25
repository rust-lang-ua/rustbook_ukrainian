# Мова Програмування Rust

[Мова Програмування Rust](title-page.md) [Передмова](foreword.md) [Вступ](ch00-00-introduction.md)

## Починаємо

- [Початок](ch01-00-getting-started.md)
    - [Встановлення](ch01-01-installation.md)
    - [Hello, World!](ch01-02-hello-world.md)
    - [Привіт, Cargo!](ch01-03-hello-cargo.md)

- [Програмування Гри Відгадайки](ch02-00-guessing-game-tutorial.md)

- [Загальні Концепції Програмування](ch03-00-common-programming-concepts.md)
    - [Змінні і Мутабельність](ch03-01-variables-and-mutability.md)
    - [Типи Даних](ch03-02-data-types.md)
    - [Функції](ch03-03-how-functions-work.md)
    - [Коментарі](ch03-04-comments.md)
    - [Потік Виконання](ch03-05-control-flow.md)

- [Розуміння Володіння](ch04-00-understanding-ownership.md)
    - [Що Таке Володіння?](ch04-01-what-is-ownership.md)
    - [Посилання та Позичання](ch04-02-references-and-borrowing.md)
    - [Слайси](ch04-03-slices.md)

- [Використання Структур для Групування Пов'язаних Даних](ch05-00-structs.md)
    - [Визначення та Створення Екземпляра Структури](ch05-01-defining-structs.md)
    - [Приклад Програми з Використанням Структур](ch05-02-example-structs.md)
    - [Синтаксис Методів](ch05-03-method-syntax.md)

- [Енуми та Зіставлення зі Шаблоном](ch06-00-enums.md)
    - [Визначення Енума](ch06-01-defining-an-enum.md)
    - [Конструкція Потоку Виконання `match`](ch06-02-match.md)
    - [Лаконічний Потік Виконання з `if let`](ch06-03-if-let.md)

## Базова Грамотність Rust

- [Керування Щораз Більшими Проєктами із Пакетами, Крейтами та Модулями](ch07-00-managing-growing-projects-with-packages-crates-and-modules.md)
    - [Пакети та Крейти](ch07-01-packages-and-crates.md)
    - [Визначення Модулів для Контролю Області Видимості та Приватності](ch07-02-defining-modules-to-control-scope-and-privacy.md)
    - [Шлях для Доступу до Елементів у Дереві Модулів](ch07-03-paths-for-referring-to-an-item-in-the-module-tree.md)
    - [Введення Шляхів до Області Видимості з Ключовим Словом use](ch07-04-bringing-paths-into-scope-with-the-use-keyword.md)
    - [Розподіл Модулів на Різні Файли](ch07-05-separating-modules-into-different-files.md)

- [Звичайні Колекції](ch08-00-common-collections.md)
    - [Зберігання Списків Значень з Векторами](ch08-01-vectors.md)
    - [Зберігання Тексту у Кодуванні UTF-8 в Стрічках](ch08-02-strings.md)
    - [Зберігання Ключів з Асоційованими Значеннями у Хеш-Мапах](ch08-03-hash-maps.md)

- [Обробка Помилок](ch09-00-error-handling.md)
    - [Невідновлювані Помилки з `panic!`](ch09-01-unrecoverable-errors-with-panic.md)
    - [Відновлювані Помилки з `Result`](ch09-02-recoverable-errors-with-result.md)
    - [`panic!` чи не `panic!`](ch09-03-to-panic-or-not-to-panic.md)

- [Узагальнені Типи, Трейти та Часи Існування](ch10-00-generics.md)
    - [Узагальнені Типи Даних](ch10-01-syntax.md)
    - [Трейти: Визначення Спільної Поведінки](ch10-02-traits.md)
    - [Перевірка Коректності Посилань із Часами Існування](ch10-03-lifetime-syntax.md)

- [Написання Автоматизованих Тестів](ch11-00-testing.md)
    - [Як Писати Тести](ch11-01-writing-tests.md)
    - [Керування Запуском Тестів](ch11-02-running-tests.md)
    - [Організація Тестів](ch11-03-test-organization.md)

- [Проєкт з Вводом/Виводом: Створення Програми з Інтерфейсом Командного Рядка](ch12-00-an-io-project.md)
    - [Приймання Аргументів Командного Рядка](ch12-01-accepting-command-line-arguments.md)
    - [Читання Файлу](ch12-02-reading-a-file.md)
    - [Рефакторинг для Покращення Модульності та Обробки Помилок](ch12-03-improving-error-handling-and-modularity.md)
    - [Розробка Функціонала Бібліотеки із Test-Driven Development](ch12-04-testing-the-librarys-functionality.md)
    - [Робота зі Змінними Середовища](ch12-05-working-with-environment-variables.md)
    - [Написання Повідомлень про Помилки у Помилковий Вивід замість Стандартного Виводу](ch12-06-writing-to-stderr-instead-of-stdout.md)

## Мислення в Стилі Rust

- [Функціональні Можливості Мови: Ітератори та Замикання](ch13-00-functional-features.md)
    - [Замикання: Анонімні Функції, що Захоплюють Своє Середовище](ch13-01-closures.md)
    - [Обробка Послідовностей Елементів з Ітераторами](ch13-02-iterators.md)
    - [Покращення Нашого Проєкту з Вводом/Виводом](ch13-03-improving-our-io-project.md)
    - [Порівняння Швидкодії: Цикли Проти Ітераторів](ch13-04-performance.md)

- [Більше про Cargo та Crates.io](ch14-00-more-about-cargo.md)
    - [Налаштування Збірок з Release Профілями](ch14-01-release-profiles.md)
    - [Публікація Крейта на Crates.io](ch14-02-publishing-to-crates-io.md)
    - [Робочі Області Cargo](ch14-03-cargo-workspaces.md)
    - [Встановлення Двійкових Файлів з `cargo install`](ch14-04-installing-binaries.md)
    - [Розширення Cargo із Користувацькими Командами](ch14-05-extending-cargo.md)

- [Розумні Вказівники](ch15-00-smart-pointers.md)
    - [Використання `Box<T>` для Вказування на Дані в Купі](ch15-01-box.md)
    - [Ставлення до Розумних Вказівників як до Звичайних Посилань з Трейтом `Deref`](ch15-02-deref.md)
    - [Виконання Коду при Очищенні з Трейтом `Drop`](ch15-03-drop.md)
    - [`Rc<T>` - Розумний Вказівник з Лічильником Посилань](ch15-04-rc.md)
    - [`RefCell<T>` та Шаблон Внутрішньої Мутабельності](ch15-05-interior-mutability.md)
    - [Цикли Посилань Можуть Спричинити Витік Пам'яті](ch15-06-reference-cycles.md)

- [Безстрашна Конкурентність](ch16-00-concurrency.md)
    - [Використання Потоків для Одночасного Виконання Коду](ch16-01-threads.md)
    - [Застосування Обміну Повідомлень для Передавання Даних між Потоками](ch16-02-message-passing.md)
    - [Конкурентність зі Спільним Станом](ch16-03-shared-state.md)
    - [Розширювана Конкурентність із Трейтами Sync та Send](ch16-04-extensible-concurrency-sync-and-send.md)

- [Особливості Об'єктоорієнтованого Програмування в Rust](ch17-00-oop.md)
    - [Характеристики Об'єктоорієнтованих Мов](ch17-01-what-is-oo.md)
    - [Використання Трейт-Об'єктів, які Допускають Значення Різних Типів](ch17-02-trait-objects.md)
    - [Реалізація Об'єктоорієнтованого Шаблону Проєктування](ch17-03-oo-design-patterns.md)

## Просунуті Питання

- [Шаблони та Зіставлення Шаблонів](ch18-00-patterns.md)
    - [Усі Місця Можливого Використання Шаблонів](ch18-01-all-the-places-for-patterns.md)
    - [Спростовуваність: Чи Може Шаблон Бути Невідповідним](ch18-02-refutability.md)
    - [Синтаксис Шаблонів](ch18-03-pattern-syntax.md)

- [Просунуті Можливості](ch19-00-advanced-features.md)
    - [Небезпечний Rust](ch19-01-unsafe-rust.md)
    - [Поглиблено про Трейти](ch19-03-advanced-traits.md)
    - [Поглиблено про Типи](ch19-04-advanced-types.md)
    - [Поглиблено про Функції та Замикання](ch19-05-advanced-functions-and-closures.md)
    - [Макроси](ch19-06-macros.md)

- [Останній Проєкт: Збірка Багатопотокового Вебсервера](ch20-00-final-project-a-web-server.md)
    - [Збірка Однопотокового Вебсервера](ch20-01-single-threaded.md)
    - [Перетворюємо Наш Однопотоковий Сервер на Багатопотоковий](ch20-02-multithreaded.md)
    - [Плавне Вимкнення та Очищення](ch20-03-graceful-shutdown-and-cleanup.md)

- [Додатки](appendix-00.md)
    - [A - Ключові Слова](appendix-01-keywords.md)
    - [B - Оператори та Символи](appendix-02-operators.md)
    - [C - Похідні Трейти](appendix-03-derivable-traits.md)
    - [D - Корисні Інструменти Розробки](appendix-04-useful-development-tools.md)
    - [E - Видання](appendix-05-editions.md)
    - [F - Переклади Книги](appendix-06-translation.md)
    - [G - як Розробляється Rust і "Нічний Rust"](appendix-07-nightly-rust.md)
