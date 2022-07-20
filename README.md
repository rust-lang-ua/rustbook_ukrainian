
# Мова програмування Rust [![Crowdin](https://badges.crowdin.net/rustukrainian/localized.svg)](https://crowdin.com/project/rustukrainian)

Оригінал цієї книжки розміщений на [rust-lang.github.io/book/][html].
Цей переклад поки що знаходиться в початковому стані, ласкаво прошу всіх 
долучатися.

[html]: http://rust-lang.github.io/book/
Це наступна ітерація книжки "Мова програмування Rust" ([сирці][src], 
[текст онлайн][prod]).

[src]: https://github.com/rust-lang/rust/tree/master/src/doc/book
[prod]: https://doc.rust-lang.org/book/

## Вимоги

Побудова цієї книжки вимагає [mdBook] >= v0.0.13. Встановити його можна так:

[mdBook]: https://github.com/azerupi/mdBook

```
$ cargo install mdbook
```

## Збирання

Щоб зібрати книжку, наберіть:

```
$ mdbook build
```

Результат буде в підтеці `book`. Для перевірки, відкрийте її у веб-переглядачі.

_Firefox:_
```
$ firefox book/index.html           # Linux
$ open -a "Firefox" book/index.html # OS X
```

_Chrome:_
```
$ google-chrome book/index.html           # Linux
$ open -a "Google Chrome" book/index.html # OS X
```

Для виконання тестів:

```
$ mdbook test
```

## Участь у проекті

Нам дуже важлива ваша допомога! Перегляньте [CONTRIBUTING.md][contrib] щоб 
дізнатися, як допомогти оригінальному проекту, і [CONTRIBUTING.md][contrib-uk] 
для допомоги цьому перекладу.

[contrib]: https://github.com/rust-lang/book/blob/master/CONTRIBUTING.md
[contrib-uk]: https://github.com/pavloslav/rust-book-ukrainian/blob/master/CONTRIBUTING.md

### Словник

Для перекладу формується невеликий [словничок][dictionary], пропозиції і 
зауваження заохочуються.

[dictionary]: https://github.com/pavloslav/rust-book-ukrainian/blob/master/DICTIONARY.md

### Інші переклади

Особливо важлива ваша допомога в перекладанні! Прогляньте повідомлення, 
позначені [Translations][], щоб приєднати свої зусилля до наявної роботи. 
Відкрийте нову тему (issue), щоб почати роботу над новою мовою! Наразі 
доведеться зачекати  на підтримку багатомовності в [mdbook][] перед злиттям
перекладів до книжки, але розпочати перекладати можна вже зараз! Розділи в 
[the frozen column][замороженій колонці] проекту вже не будуть радикально 
змінені, тому якщо ви почнете з них, вам не доведеться переробляти зроблене :)

[Translations]: https://github.com/rust-lang/book/issues?q=is%3Aopen+is%3Aissue+label%3ATranslations
[mdbook]: https://github.com/azerupi/mdBook/issues/5
[the frozen column]: https://github.com/rust-lang/book/projects/1

## Український переклад у вебі
https://pavloslav.github.io/RustBookUkr/
