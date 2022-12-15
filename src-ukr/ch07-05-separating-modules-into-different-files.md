## Розподіл модулів на різні файли

Поки що всі приклади в цій главі визначали декілька модулів у одному файлі. Коли модулі стають великими, ви можете захотіти перемістити їх визначення в окремі файли, щоб спростити навігацію по коду.

Наприклад, почнімо з коду із Лістинга 7-17, у якому було декілько модулів ресторану. Ми будемо вилучати модулі у файли замість того, щоб визначати всі модулі в кореневому модулі крейта. У нашому випадку кореневий модуль крейта - *src/lib.rs*, але цей розподіл також працює з бінарними крейтами, у яких кореневий модуль крейта - *src/main.rs*.

Спочатку ми вилучимо модуль `front_of_house` в свій власний файл. Видаліть код всередині фігурних дужок для модуля `front_of_house`, залишив тільки визначення `mod front_of_house;` так щоб тепер *src/lib.rs* містив код, показаний в Лістингу 7-21. Зверніть увагу, що цей варіант не скомпілюється, поки ми не створимо файл *src/front_of_house.rs* з Лістинга 7-22.

<span class="filename">Файл: src/lib.rs</span>

```rust,ignore,does_not_compile
{{#rustdoc_include ../listings/ch07-managing-growing-projects/listing-07-21-and-22/src/lib.rs}}
```


<span class="caption">Listing 7-21: Declaring the `front_of_house` module whose body will be in *src/front_of_house.rs*</span>

Далі, розмістимо код, котрий був у фігурних дужках, у новий файл з ім'ям *src/front_of_house.rs*, як показано у Лістингу 7-22. Компілятор знає, що потрібно шукати у цьому файлі, тому що він натрапив у кореневому модулі крейту на визначення модуля з ім'ям `front_of_house`.

<span class="filename">Файл: src/front_of_house.rs</span>

```rust,ignore
{{#rustdoc_include ../listings/ch07-managing-growing-projects/listing-07-21-and-22/src/front_of_house.rs}}
```


<span class="caption">Listing 7-22: Definitions inside the `front_of_house` module in *src/front_of_house.rs*</span>

Зверніть увагу, що вам потрібно тільки `один` раз завантажити файл за допомогою оголошення *mod* у вашому дереві модулів. Як тільки компілятор дізнається, що файл є частиною проекта (та дізнається, де в дереві модулей знаходиться код за допомогою того, де ви розмістили оператор `mod`), інші файли у вашому проекті повинні посилатися на код завантаженого файлу, використовуючи шлях до місця, де він був оголошений, як описано у секції [Шляхи для посилання на елемент у дереві модулів][paths]<!-- ignore --> . Іншими словами, `mod` - це *не* операція “включення”, яку ви могли бачати в інших мовах програмування.

Далі ми вилучимо модуль `hosting` в його власний файл. Процес трохи відрізняється, тому що `hosting` є дочірнім модулем для `front_of_house`, а не кореневого модуля. Ми помістимо файл для `hosting` в нову директорію, який буде іменований на ім'я його предка в дереві модулів, у цьому випадку це *src/front_of_house/*.

To start moving `hosting`, we change *src/front_of_house.rs* to contain only the declaration of the `hosting` module:

<span class="filename">Файл: src/front_of_house.rs</span>

```rust,ignore
{{#rustdoc_include ../listings/ch07-managing-growing-projects/no-listing-02-extracting-hosting/src/front_of_house.rs}}
```

Then we create a *src/front_of_house* directory and a file *hosting.rs* to contain the definitions made in the `hosting` module:

<span class="filename">Файл: src/front_of_house/hosting.rs</span>

```rust,ignore
{{#rustdoc_include ../listings/ch07-managing-growing-projects/no-listing-02-extracting-hosting/src/front_of_house/hosting.rs}}
```

Якщо замість цього ми розмістимо *hosting.rs* у директорію *src*, компілятор буде думати, що код в *hosting.rs* це модуль `hosting`, визначений у корні крейта, а не визначений як дочірній модуль `front_of_house`. Правила компілятору для перевірки того, які файли містять код яких модулів, припускають, що директорії та файли точно відповідають дереву модулів.

> ### Альтернативні шляхи до файлів
> 
> Досі ми розглядали найбільш ідіоматичні шляхи до файлів, які використовуються компілятором Rust, але Rust також підтримує старий стиль шляхів до файлу. Для модуля з ім'ям `front_of_house` визначеного в кореневому модулі крейту, компілятор буде шукати код модуля в:
> 
> * *src/front_of_house.rs* (стиль, що ми розглядали)
> * *src/front_of_house/mod.rs* (старий стиль, який все ще підтримується)
> 
> For a module named `hosting` that is a submodule of `front_of_house`, the compiler will look for the module’s code in:
> 
> * *src/front_of_house/hosting.rs* (стиль, що ми розглядали)
> * *src/front_of_house/hosting/mod.rs* (старий стиль, який все ще підтримується)
> 
> Якщо ви використовуєте обидва стилі для одного й того ж модуля, ви отримаєте помилку компілятора. Використання суміші обох стилів для різних модулів у одному проекті дозволено, але це може збивати з пантелику людей, що переміщаються по вашому проекту.
> 
> The main downside to the style that uses files named *mod.rs* is that your project can end up with many files named *mod.rs*, which can get confusing when you have them open in your editor at the same time.

Ми перенесли код кожного модуля в окремий файл, а дерево модулів залишилось без змін. Виклики функцій в `eat_at_restaurant` будуть працювати без яких-небудь змін, не дивлячись на те, що визначення знаходяться у різних файлах. Цей метод дозволяє переміщати модулі в нові файли в міру збільшення їх розмірів.

Зверніть увагу, що оператор `pub use crate::front_of_house::hosting` у *src/lib.rs* також не змінився, та `use` не впливає на те, які файли компілюються як частина крейта. Ключове слово `mod` визначає модулі, і Rust шукає в файлі з таким же ім'ям, що й у модуля, який входить у цей модуль.

## Підсумок

Rust дозволяє розбити пакет на декілька крейтів, та крейт - на модулі, таким чином ви маєте змогу посилатися на елементи, визначенні в одному модулі, з іншого модуля. Це можна робити за допомогою вказання абсолютних чи відносних шляхів. Ці шляхи можна додати в область видимості оператором `use`, тому ми можете користуватися коротшими шляхами для багаторазового використання елементів у цій області видимості. Код модуля за замовчуванням є приватним, але можна зробити визначення загальнодоступними, додавши ключове слово `pub`.

In the next chapter, we’ll look at some collection data structures in the standard library that you can use in your neatly organized code.

[paths]: ch07-03-paths-for-referring-to-an-item-in-the-module-tree.html
