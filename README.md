# 🚀 Настройка C++ в VS Code на Windows 11

[![Platform](https://img.shields.io/badge/Platform-Windows%2011-lightgrey?style=flat-square)]()
[![License](https://img.shields.io/badge/License-MIT-yellow?style=flat-square)](LICENSE)
[![PRs Welcome](https://img.shields.io/badge/PRs-welcome-brightgreen?style=flat-square)](CONTRIBUTING.md)

> **Полное пошаговое руководство** по созданию современной среды разработки C++ на Windows 11 с нуля.

---

<a id="toc"></a>
## 📋 Оглавление

- [ Введение](#introduction)
- [ Требования](#requirements)
- [0. Проверка архитектуры системы](#step-0)
- [1. Установка MSYS2](#step-1)
- [2. Установка компилятора и отладчика](#step-2)
- [3. Настройка переменных среды (PATH)](#step-3)
- [4. Установка VS Code и базовых расширений](#step-4)
- [5. Настройка IntelliSense (C/C++ Extension)](#step-5)
- [6. Настройка сборки и отладки](#step-6)
- [7. Установка и настройка Git Bash](#step-7)
- [8. Интеграция с GitHub в VS Code](#step-8)
- [9. Продвинутая настройка: CMake и CMake Tools](#step-9)
- [💡 Полезные фишки и горячие клавиши](#tips)
- [🔧 Устранение неполадок](#troubleshooting)
- [🤝 Как внести вклад](#contributing)
- [📄 Лицензия](#license)

---

<a id="introduction"></a>
## 📖 Введение

Это руководство — практический путеводитель по настройке полноценной среды разработки C++ на **Windows 11** с использованием **MSYS2**, компилятора **GCC/G++**, отладчика **GDB**, редактора **Visual Studio Code**, системы сборки **CMake** и системы контроля версий **Git**. Инструкция проверена на реальных проектах и содержит все нюансы, которые обычно всплывают уже после установки.

**Что вы получите в итоге:**

| Компонент | Назначение |
|---|---|
| MSYS2 UCRT64 | Современное окружение с актуальными пакетами |
| GCC / G++ 13+ | Компилятор с поддержкой C++17/20 |
| GDB 13+ | Отладчик с pretty-printing контейнеров STL |
| Visual Studio Code | Редактор с IntelliSense и встроенной отладкой |
| CMake + Ninja | Промышленная система сборки |
| Git Bash + GitHub | Контроль версий и публикация проектов |

**Для кого это руководство:** для разработчиков, которые хотят получить рабочую IDE «из коробки» без сюрпризов с путями, флагами компилятора и конфигурациями `tasks.json`/`launch.json`.

**Время прохождения:** ~45–60 минут (зависит от скорости интернета).

---

<a id="requirements"></a>
## ✅ Требования

Перед началом убедитесь, что ваша система соответствует следующим критериям:

| Требование | Минимум | Рекомендуется |
|---|---|---|
| ОС | Windows 10 (1809+) | Windows 11 (последняя стабильная) |
| Архитектура | x64 или ARM64 | x64 (Intel/AMD) |
| Свободное место на диске | 2 ГБ | 5 ГБ+ (с учётом кэша пакетов) |
| Оперативная память | 4 ГБ | 8 ГБ+ |
| Права | Администратор (для установки) | Администратор |
| Интернет | Обязателен | Стабильный宽带 |
| Аккаунт GitHub | Не обязателен для начала | Настоятельно рекомендуется |
| Базовые знания | Командная строка, установка программ | Опыт работы с C++ |

**Уже установленные программы:** если ранее ставили MinGW, Cygwin или другую сборку GCC — **удалите их или уберите из `PATH`**, чтобы избежать конфликтов с MSYS2.

---

<a id="step-0"></a>
## Шаг 0: Проверка архитектуры системы

Перед загрузкой любого установщика необходимо убедиться в архитектуре процессора. Windows 11 работает как на **x64** (Intel/AMD), так и на **ARM64** (Snapdragon и др.).

**Как проверить:**

1. Нажмите `Win + I`, чтобы открыть **Параметры**.
2. Перейдите в раздел **Система** → **О системе**.
3. Найдите строку **«Тип системы»**:
    - **«64-разрядная операционная система, процессор x64»** → архитектура **x64**.
    - **«64-разрядная операционная система, процессор на базе ARM»** → архитектура **ARM64**.

> **💡 Важное примечание для ARM64:** MSYS2 имеет отдельную версию для ARM64 (`clangarm64`), но экосистема C++ с GCC/GDB там менее развита. Для большинства пользователей ARM64 рекомендуется использовать окружение **`clang64`** в MSYS2, которое отлично поддерживает компилятор Clang и отладчик LLDB. Инструкция ниже ориентирована на **x64**, но принципы настройки VS Code останутся теми же (пути к компилятору/отладчику нужно будет заменить на `clang`/`lldb`).

---

<a id="step-1"></a>
## Шаг 1: Установка MSYS2

MSYS2 — это среда, предоставляющая доступ к репозиторию пакетов (включая GCC и GDB) и эмуляцию Unix-окружения.

**Актуальная версия:** На момент написания руководства последняя стабильная версия — **2026-09-27** (релиз `msys2-x86_64-20260927.exe`). Проверьте актуальную версию на [msys2.org](https://www.msys2.org/).

1. **Скачайте установщик:** Перейдите на официальный сайт [msys2.org](https://www.msys2.org/) и скачайте установщик `msys2-x86_64-...exe` (для x64).
2. **Запустите установку:** Рекомендуется оставить путь по умолчанию — `C:\msys64`. Это короткий ASCII-путь без пробелов, что избавит от многих проблем.
3. **Завершение:** **Снимите галочку** с пункта «Run MSYS2 Now» в конце установки. Мы сделаем это вручную, чтобы сразу попасть в нужное окружение.

---

<a id="step-2"></a>
## Шаг 2: Установка компилятора и отладчика

1. Откройте **MSYS2 UCRT64** из меню «Пуск». Это окружение использует современную библиотеку времени выполнения Universal C Runtime (UCRT).
2. **Опционально (для скорости):** Смените зеркало на более близкое (например, на USTC) и обновите индекс пакетов:
    ```bash
    sed -i "s#mirror.msys2.org/#mirrors.ustc.edu.cn/msys2/#g" /etc/pacman.d/mirrorlist*
    ```
3. Обновите базу пакетов:
    ```bash
    pacman -Syu
    ```
    Терминал может закрыться после обновления — это нормально. Откройте его снова.
4. Установите полный набор инструментов:
    ```bash
    pacman -S --needed base-devel mingw-w64-ucrt-x86_64-toolchain
    ```
    - `--needed` — пропустит уже установленные пакеты.
    - `base-devel` — базовые инструменты разработки.
    - `mingw-w64-ucrt-x86_64-toolchain` — группа, включающая `gcc`, `g++`, `gdb` и другие утилиты.

    Во время установки нажимайте `Enter` для подтверждения.

---

<a id="step-3"></a>
## Шаг 3: Настройка переменных среды (PATH)

**Это критически важный шаг**, который часто пропускают. Чтобы VS Code и другие инструменты могли находить компилятор, путь к нему нужно добавить в системную переменную `PATH`.

1. В поиске Windows начните вводить «**Изменение системных переменных среды**» и откройте это окно.
2. Нажмите кнопку **«Переменные среды...»**.
3. В верхнем блоке («Переменные среды пользователя») найдите переменную **`Path`** и нажмите **«Изменить...»**.
4. Нажмите **«Создать»** и добавьте путь:
    ```
    C:\msys64\ucrt64\bin
    ```
    (Если вы установили MSYS2 в другое место, укажите свой путь).
5. Нажмите «ОК» во всех трёх окнах, чтобы сохранить изменения.

> **⚠️ Внимание:** Если в вашем `PATH` уже есть другие компиляторы (например, `C:\MinGW\bin` или `C:\mingw64\bin`), они могут конфликтовать с MSYS2. Убедитесь, что путь `C:\msys64\ucrt64\bin` находится **вверху** списка, или удалите старые записи.

**Проверка:** Откройте **новое** окно командной строки (`cmd`) или PowerShell и выполните:
```bash
gcc --version
g++ --version
gdb --version
```
Вы должны увидеть информацию о версиях. Если команды не найдены, перезагрузите компьютер или проверьте правильность добавленного пути.

---

<a id="step-4"></a>
## Шаг 4: Установка VS Code и базовых расширений

1. **Скачайте VS Code:** Перейдите на [code.visualstudio.com](https://code.visualstudio.com/) и скачайте установщик для Windows (архитектура `x64` или `Arm64` в соответствии с Шагом 0).
2. **Установите VS Code:** При установке рекомендуется отметить пункты «**Добавить действие "Открыть с помощью Code" в контекстное меню проводника**» для файлов и папок.
3. **Установите ключевые расширения.** Откройте VS Code, нажмите `Ctrl+Shift+X` и установите:

| Расширение | Идентификатор | Назначение |
|---|---|---|
| **C/C++** | `ms-vscode.cpptools` | Основное расширение: IntelliSense, отладка, навигация по коду |
| **CMake Tools** | `ms-vscode.cmake-tools` | Интеграция с системой сборки CMake |
| **GitLens** | `eamodio.gitlens` | (Рекомендуется) Расширенная работа с Git прямо в редакторе |
| **Error Lens** | `usernamehw.errorlens` | (Рекомендуется) Показ ошибок компилятора прямо в строках кода |

---

<a id="step-5"></a>
## Шаг 5: Настройка IntelliSense (C/C++ Extension)

IntelliSense обеспечивает автодополнение, подсказки и проверку ошибок в реальном времени.

1. Создайте папку проекта, например `C:\Projects\HelloWorld`, и откройте её в VS Code (`Файл` → `Открыть папку...`).
2. Создайте файл `main.cpp` с простым кодом:
    ```cpp
    #include <iostream>

    int main() {
        int a = 5;
        int b = 10;
        int sum = a + b;
        std::cout << "Sum is: " << sum << std::endl;
        return 0;
    }
    ```
3. **Настройте IntelliSense:**
    - Нажмите `Ctrl+Shift+P` → **`C/C++: Edit Configurations (UI)`**.
    - **Compiler path:** `C:\msys64\ucrt64\bin\g++.exe`
    - **IntelliSense mode:** `windows-gcc-x64`
    - **C++ standard:** `c++17` или `c++20` (на ваш выбор)
    - Настройки сохранятся в `.vscode/c_cpp_properties.json`.

---

<a id="step-6"></a>
## Шаг 6: Настройка сборки и отладки

### 6.1. Настройка сборки (`tasks.json`)

1. `Ctrl+Shift+P` → **`Tasks: Configure Default Build Task`** → **`C/C++: g++.exe build active file`**.
2. Убедитесь, что в аргументах есть флаг **`-g`** — без него GDB не сможет показать значения переменных.

```json
{
    "version": "2.0.0",
    "tasks": [
        {
            "type": "cppbuild",
            "label": "C/C++: g++.exe build active file",
            "command": "C:\\msys64\\ucrt64\\bin\\g++.exe",
            "args": [
                "-fdiagnostics-color=always",
                "-g",
                "${file}",
                "-o",
                "${fileDirname}\\${fileBasenameNoExtension}.exe"
            ],
            "options": { "cwd": "${fileDirname}" },
            "problemMatcher": ["$gcc"],
            "group": { "kind": "build", "isDefault": true },
            "detail": "Task generated by Debugger."
        }
    ]
}
```

### 6.2. Настройка отладки (`launch.json`)

1. `Ctrl+Shift+P` → **`Debug: Add Configuration`** → **`C++ (GDB/LLDB)`** → **`g++.exe build and debug active file`**.
2. Ключевой параметр — `"miDebuggerPath"`. Он должен указывать на `gdb.exe`.

```json
{
    "version": "0.2.0",
    "configurations": [
        {
            "name": "C/C++: g++.exe build and debug active file",
            "type": "cppdbg",
            "request": "launch",
            "program": "${fileDirname}\\${fileBasenameNoExtension}.exe",
            "args": [],
            "stopAtEntry": false,
            "cwd": "${fileDirname}",
            "environment": [],
            "externalConsole": false,
            "MIMode": "gdb",
            "miDebuggerPath": "C:\\msys64\\ucrt64\\bin\\gdb.exe",
            "setupCommands": [
                {
                    "description": "Enable pretty-printing for gdb",
                    "text": "-enable-pretty-printing",
                    "ignoreFailures": true
                }
            ],
            "preLaunchTask": "C/C++: g++.exe build active file"
        }
    ]
}
```

> **💡 Фишка:** Для программ, использующих консольный ввод (`cin`), установите `"externalConsole": true`. Иначе ввод может не работать во встроенном терминале отладчика.

3. **Проверка:** Поставьте точку останова (кликните слева от строки `int sum = a + b;`), нажмите `F5`. Программа остановится, и вы увидите значения переменных в панели «VARIABLES».

---

<a id="step-7"></a>
## Шаг 7: Установка и настройка Git Bash

1. **Скачайте Git:** [git-scm.com/downloads](https://git-scm.com/downloads).
2. **Установите Git.** Важные опции:
    - **«Select Components»** → отметьте **«Add a Git Bash Profile to Windows Terminal»**.
    - **«Adjusting your PATH environment»** → выберите **«Git from the command line and also from 3rd-party software»**.
3. **Настройте Git** (в Git Bash):
    ```bash
    git config --global user.name "Your Name"
    git config --global user.email "your@email.com"
    git config --global core.editor "code --wait"
    git config --global init.defaultBranch main
    ```
4. **Интеграция Git Bash в VS Code:**
    - `Ctrl+Shift+P` → **`Terminal: Select Default Profile`** → выберите **`Git Bash`**.
    - Теперь `Ctrl+`` будет открывать Git Bash.

---

<a id="step-8"></a>
## Шаг 8: Интеграция с GitHub в VS Code

1. **Войдите в GitHub:** Иконка «Аккаунты» в левом нижнем углу → «Войти с помощью GitHub».
2. **Инициализация репозитория:** Панель **Source Control** (`Ctrl+Shift+G`) → **Initialize Repository**.
3. **Первый коммит:** Введите сообщение (например, «Initial commit») → нажмите галочку (Commit).
4. **Публикация на GitHub:** Нажмите **Publish Branch**, выберите тип репозитория (public/private) — VS Code автоматически создаст репозиторий и запушит код.

---

<a id="step-9"></a>
## Шаг 9: Продвинутая настройка: CMake и CMake Tools

Для проектов из нескольких файлов ручная настройка `tasks.json` неудобна. Используйте **CMake**.

1. **Установите расширение:** `ms-vscode.cmake-tools`.
2. **Установите CMake и Ninja в MSYS2:**
    ```bash
    pacman -S mingw-w64-ucrt-x86_64-cmake mingw-w64-ucrt-x86_64-ninja
    ```
3. **Создайте `CMakeLists.txt`** в корне проекта:
    ```cmake
    cmake_minimum_required(VERSION 3.10)
    project(HelloWorld)

    set(CMAKE_CXX_STANDARD 17)
    set(CMAKE_CXX_STANDARD_REQUIRED True)

    add_executable(HelloWorld main.cpp)
    ```
4. **Сконфигурируйте:** `Ctrl+Shift+P` → **`CMake: Configure`** → выберите компилятор `GCC ...` из MSYS2. Появится папка `build`.
5. **Сборка и отладка:** В статус-баре появятся кнопки **Build** и **Debug** (или `F5`). CMake Tools сгенерирует корректную конфигурацию автоматически.

---

<a id="tips"></a>
## 💡 Полезные фишки и горячие клавиши

| Комбинация | Действие |
|---|---|
| `Ctrl+Shift+P` | Палитра команд |
| `Ctrl+P` | Быстрое открытие файла по имени |
| `F5` | Запуск отладки |
| `Ctrl+F5` | Запуск без отладки |
| `Shift+Alt+F` | Форматирование кода |
| `` Ctrl+` `` | Встроенный терминал |
| `Ctrl+Shift+V` | Предпросмотр Markdown |
| `Ctrl+K, Ctrl+0` | Свернуть все блоки кода |
| `Alt+↑ / Alt+↓` | Перемещение строки |

**Полезные настройки в `settings.json`:**

```json
{
    "files.autoSave": "onFocusChange",
    "editor.formatOnSave": true,
    "editor.tabSize": 4,
    "editor.rulers": [100],
    "C_Cpp.default.cppStandard": "c++17",
    "C_Cpp.default.intelliSenseMode": "windows-gcc-x64"
}
```

---

<a id="troubleshooting"></a>
## 🔧 Устранение неполадок

| Проблема | Возможная причина | Решение |
|---|---|---|
| `gcc: command not found` | Путь к MSYS2 не добавлен в `PATH` | Выполните Шаг 3, перезапустите VS Code |
| IntelliSense не работает | Неверный путь к компилятору | Проверьте «Compiler path» в `C/C++: Edit Configurations (UI)` |
| Отладчик не останавливается на точке останова | Отсутствует флаг `-g` | Добавьте `-g` в `tasks.json` или `CMakeLists.txt` |
| `miDebuggerPath` не найден | Неверный путь к `gdb.exe` | Выполните `which gdb` в MSYS2 UCRT64 и скопируйте путь |
| Программа с `cin` не работает при отладке | `"externalConsole": false` | Установите `"externalConsole": true` |
| CMake Configure завершается ошибкой | Не установлены CMake или Ninja | `pacman -S mingw-w64-ucrt-x86_64-cmake mingw-w64-ucrt-x86_64-ninja` |
| Терминал не открывается в VS Code | Не выбран профиль терминала | `Terminal: Select Default Profile` → выберите Git Bash |

---

<a id="contributing"></a>
## 🤝 Как внести вклад

Мы приветствуем улучшения и исправления!

1. **Форкните** репозиторий.
2. **Создайте ветку** для вашей функции: `git checkout -b feature/amazing-feature`.
3. **Внесите изменения** и закоммитьте: `git commit -m 'Add some amazing feature'`.
4. **Отправьте в ветку:** `git push origin feature/amazing-feature`.
5. **Откройте Pull Request** с описанием изменений.

**Правила:**

- Проверяйте изменения локально перед PR.
- Соблюдайте единый стиль оформления README.
- Один PR — одна логическая правка.
- Следуйте [кодексу поведения](CODE_OF_CONDUCT.md).

**Нашли ошибку или неточность?** [Откройте Issue](../../issues/new) с описанием проблемы и вашей конфигурацией (ОС, версия MSYS2, версия VS Code).

---

<a id="license"></a>

## Лицензия

Распространяется под лицензией **MIT**. Смотрите файл [LICENSE](LICENSE) для подробностей.

</div>

**⭐ Если это руководство было полезным, поставьте звезду репозиторию!**  
**📫 По вопросам и предложениям создавайте Issue или обращайтесь к автору.**
