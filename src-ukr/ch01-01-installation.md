## Встановлення

Наш перший крок - встановити Rust. Ми завантажимо Rust за допомогою `rustup`, інструмента командного рядка для керування виданнями Rust і пов'язаних інструментів. Для завантаження вам знадобиться з'єднання з Інтернетом.

> Note: If you prefer not to use `rustup` for some reason, please see the [Other Rust Installation Methods page][otherinstall] for more options.

Наступні кроки встановлять найостаннішу стабільну версію компілятора Rust. Принципи стабільності Rust гарантують, що всі приклади в цій книжці, які можна скомпілювати, будуть компілюватися в новіших версіях Rust. Повідомлення можуть незначно змінюватися від версії до версії, бо Rust часто покращує повідомлення і попередження про помилки. Іншими словами, будь-яка новіша стабільна версія Rust, яку ви встановите за цією інструкцією, має працювати відповідно до змісту цієї книжки.

> ### Запис у командному рядку
> 
> У цьому розділі та надалі в книжці ми використовуватимемо команди термінала. Рядки, що треба вводити в термінал, починаються з `$`. Не треба вводити сам  символ `$`; це запрошення командного рядка, що лише позначає початок команди. Рядки, що не починаються з `$` зазвичай показують те, що виводить попередня команда. Additionally, PowerShell-specific examples will use `>` rather than `$`.

### Встановлення `rustup` на Linux або macOs

Якщо ви користувач Linux або macOS, відкрийте термінал і введіть цю команду:

```console
$ curl --proto '=https' --tlsv1.2 https://sh.rustup.rs -sSf | sh
```

Ця команда завантажить сценарій і почне встановлення інструменту `rustup`, що встановить останню стабільну версію Rust. Можливо, у вас запитають ваш пароль. Якщо встановлення буде успішним, з'явиться цей рядок:

```text
Rust is installed now. Great!
```

Крім того, вам знадобиться якийсь *компонувальник (linker)*, тобто програма, яку Rust використовує, щоб об'єднати результати компіляції в один файл. Швидше за все, він уже встановлений. Якщо ви отримаєте повідомлення про помилки компонувальника, вам слід встановити компілятор C, який зазвичай включає компонувальник. Компілятор C також корисний, бо деякі поширені пакунки Rust залежать від коду на C і потребуватимуть компілятора C.

На macOS, ви можете отримати C компілятор, виконавши команду:

```console
$ xcode-select --install
```

Користувачі Linux зазвичай мають встановлювати GCC або Clang, відповідно до документації свого дистрибутиву. Скажімо, якщо ви використовуєте Ubuntu, ви можете встановити пакунок `build-essential`.

### Встановлення `rustup` на Windows

На Windows, перейдіть до [https://www.rust-lang.org/tools/install][install] і дотримуйтеся вказаних там інструкцій для встановлення Rust. У певний момент встановлення ви отримаєте повідомлення, що вам також знадобляться інструменти збірки MSVC для Visual Studio 2013 чи пізнішої.

To acquire the build tools, you’ll need to install [Visual Studio 2022][visualstudio]. When asked which workloads to install, include:

* “Desktop Development with C++”
* SDK для Windows 10 чи 11
* The English language pack component, along with any other language pack of your choosing

The rest of this book uses commands that work in both *cmd.exe* and PowerShell. If there are specific differences, we’ll explain which to use.

### Вирішення проблем

To check whether you have Rust installed correctly, open a shell and enter this line:

```console
$ rustc --version
```

You should see the version number, commit hash, and commit date for the latest stable version that has been released, in the following format:

```text
rustc x.y.z (abcabcabc yyyy-mm-dd)
```

If you see this information, you have installed Rust successfully! If you don’t see this information, check that Rust is in your `%PATH%` system variable as follows.

In Windows CMD, use:

```console
> echo %PATH%
```

In PowerShell, use:

```powershell
> echo $env:Path
```

In Linux and macOS, use:

```console
$ echo $PATH
```

If that’s all correct and Rust still isn’t working, there are a number of places you can get help. Find out how to get in touch with other Rustaceans (a silly nickname we call ourselves) on [the community page][community].

### Оновлення та видалення

Once Rust is installed via `rustup`, updating to a newly released version is easy. From your shell, run the following update script:

```console
$ rustup update
```

To uninstall Rust and `rustup`, run the following uninstall script from your shell:

```console
$ rustup self uninstall
```

### Локальна документація

The installation of Rust also includes a local copy of the documentation so that you can read it offline. Run `rustup doc` to open the local documentation in your browser.

Any time a type or function is provided by the standard library and you’re not sure what it does or how to use it, use the application programming interface (API) documentation to find out!

[otherinstall]: https://forge.rust-lang.org/infra/other-installation-methods.html
[install]: https://www.rust-lang.org/tools/install
[visualstudio]: https://visualstudio.microsoft.com/downloads/
[community]: https://www.rust-lang.org/community
