# Звичайні колекції

Стандартна бібліотека Rust містить декілька дуже корисних структур даних, що звуться *колекції*. Більшість інших типів даних представляють одне певне значення, але колекції можуть містити багато значень. На відміну від вбудованих типів масив і кортеж, дані, на які вказують ці колекції, зберігаються на купі, тобто кількість даних не має бути обов'язково відомою під час компіляції і може збільшуватися або скорочуватися під час виконання програми. Кожен вид колекції має різні можливості і недоліки, і вибір відповідної колекції для поточної ситуації - це вміння, що ви розвиваєте з часом. У цьому розділі ми обговоримо три колекції, які дуже часто використовуються в програмах Rust:

* A *vector* allows you to store a variable number of values next to each other.
* A *string* is a collection of characters. We’ve mentioned the `String` type previously, but in this chapter we’ll talk about it in depth.
* A *hash map* allows you to associate a value with a particular key. It’s a particular implementation of the more general data structure called a *map*.

To learn about the other kinds of collections provided by the standard library, see [the documentation][collections].

We’ll discuss how to create and update vectors, strings, and hash maps, as well as what makes each special.

[collections]: ../std/collections/index.html
