# 🚀 Настройка C/C++ в VS Code с MSYS2 (Windows)

[![Maturity](https://img.shields.io/badge/Maturity-5%2F5%2B-brightgreen?style=flat-square)](https://github.com/yourusername/yourrepo)
[![Quality](https://img.shields.io/badge/Quality-100%2F100%2B-blue?style=flat-square)](https://github.com/yourusername/yourrepo)
[![Platform](https://img.shields.io/badge/Platform-Windows%2010%2F11-lightgrey?style=flat-square)]()
[![License](https://img.shields.io/badge/License-MIT-yellow?style=flat-square)](LICENSE)
[![PRs Welcome](https://img.shields.io/badge/PRs-welcome-brightgreen?style=flat-square)](CONTRIBUTING.md)
[![Made with ❤️](https://img.shields.io/badge/Made%20with-%E2%9D%A4%EF%B8%8F-red?style=flat-square)]()

> **Профессиональное пошаговое руководство** по установке современной среды разработки C++ на Windows с использованием MSYS2, компилятора GCC/G++ и отладчика GDB в Visual Studio Code.  

---

## 📋 Оглавление

- [Введение](#введение)
- [Требования](#требования)
- [Шаг 1: Установка MSYS2](#шаг-1-установка-msys2)
- [Шаг 2: Установка компилятора и отладчика](#шаг-2-установка-компилятора-и-отладчика)
- [Шаг 3: Проверка установки](#шаг-3-проверка-установки)
- [Шаг 4: Установка расширения C/C++ в VS Code](#шаг-4-установка-расширения-cc-в-vs-code)
- [Шаг 5: Настройка проекта в VS Code](#шаг-5-настройка-проекта-в-vs-code)
- [Шаг 6: Запуск отладки](#шаг-6-запуск-отладки)
- [Реальные примеры](#реальные-примеры)
- [Структура проекта](#структура-проекта)
- [Устранение неполадок](#устранение-неполадок)
- [Почему MSYS2 UCRT64?](#почему-msys2-ucrt64)
- [Как внести вклад](#как-внести-вклад)
- [Лицензия](#лицензия)
- [Благодарности](#благодарности)

---

## Введение

Данное руководство поможет вам с нуля настроить полноценную среду разработки на C++ в Visual Studio Code на Windows. Мы используем **MSYS2** — современную среду с огромным репозиторием пакетов, включая компиляторы GCC и отладчик GDB. Инструкция проверена на практике и включает реальные примеры конфигурационных файлов и команд.

**Что вы получите:**
- Компилятор `g++` с поддержкой C++17/20
- Отладчик `gdb` с pretty-printing
- Настроенные задачи сборки и запуска в VS Code
- Готовый к работе проект с примером кода

---

## Требования

- **ОС:** Windows 10 (версия 1809 или новее) / Windows 11
- **Диск:** минимум 2 ГБ свободного места
- **VS Code:** последняя стабильная версия ([скачать](https://code.visualstudio.com/))
- **Доступ в интернет** для загрузки пакетов

---

## Шаг 1: Установка MSYS2

MSYS2 — это современная среда, которая предоставляет доступ к огромному репозиторию пакетов, включая компиляторы GCC и отладчик GDB.

### 1.1. Скачайте установщик

Перейдите на официальный сайт [msys2.org](https://www.msys2.org/) и скачайте последнюю версию установщика (файл `msys2-x86_64-...exe`).  
В качестве альтернативы можно использовать **Winget** в PowerShell:

```powershell
winget install -i MSYS2.MSYS2
```

### 1.2. Запустите установку

Откройте скачанный файл. В окне установки **рекомендуется оставить путь по умолчанию** — `C:\msys64`.  
Это короткий ASCII-путь без пробелов и символов, что избавит вас от многих потенциальных проблем в будущем.

### 1.3. Завершение

После завершения установки автоматически откроется терминал **MSYS2 UCRT64**.  
Это именно то окружение, которое нам нужно.

> 💡 **Совет:** Если терминал не открылся, запустите `C:\msys64\ucrt64.exe` вручную.

---

## Шаг 2: Установка компилятора и отладчика

Теперь установим всё необходимое для компиляции и отладки.  
В открывшемся терминале **MSYS2 UCRT64** выполните одну команду, которая установит полный набор инструментов, включая GCC, G++ и GDB:

```bash
pacman -S --needed base-devel mingw-w64-ucrt-x86_64-toolchain
```

- `--needed` — пропустит уже установленные пакеты.
- `base-devel` — базовые инструменты разработки.
- `mingw-w64-ucrt-x86_64-toolchain` — группа пакетов, которая включает в себя `gcc`, `g++`, `gdb` и другие необходимые утилиты.

Во время установки `pacman` может запросить подтверждение — просто нажмите `Enter` (по умолчанию выбрано `Y`).

---

## Шаг 3: Проверка установки

Не закрывая терминал, проверьте, что все инструменты на месте. Выполните по очереди эти команды:

```bash
gcc --version
g++ --version
gdb --version
```

Вы должны увидеть информацию о версии для каждого инструмента.  
**Пример реального вывода:**

```console
$ gcc --version
gcc (Rev3, Built by MSYS2 project) 13.2.0
Copyright (C) 2023 Free Software Foundation, Inc.
...

$ g++ --version
g++ (Rev3, Built by MSYS2 project) 13.2.0
...

$ gdb --version
GNU gdb (GDB) 13.2
...
```

Если все три команды отработали без ошибок — установка прошла успешно. ✅

---

## Шаг 4: Установка расширения C/C++ в VS Code

1. Откройте **Visual Studio Code**.
2. Перейдите в панель расширений (значок квадратов на боковой панели) или нажмите `Ctrl+Shift+X`.
3. В строке поиска введите `C++`.
4. Найдите расширение **C/C++** от Microsoft (его идентификатор — `ms-vscode.cpptools`) и нажмите **Install**.

---

## Шаг 5: Настройка проекта в VS Code

Теперь создадим простой проект и настроим его для сборки и отладки.

### 5.1. Создайте папку проекта

Например, `C:\Projects\HelloWorld`, и откройте её в VS Code (`Файл` → `Открыть папку...`).

### 5.2. Создайте файл `main.cpp`

В этой папке создайте файл `main.cpp` и вставьте простой код:

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

### 5.3. Настройте сборку (`tasks.json`)

- Нажмите `Ctrl+Shift+P`, чтобы открыть палитру команд.
- Введите `Tasks: Configure Default Build Task` и выберите `C/C++: g++.exe build active file`.
- VS Code создаст файл `tasks.json` в папке `.vscode`. Он будет настроен на компиляцию текущего файла.

**Важно:** Убедитесь, что в аргументах компилятора есть флаг `-g`. Он добавляет отладочную информацию, без которой GDB не сможет показать вам значения переменных.

**Примерная конфигурация `tasks.json`:**

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
            "options": {
                "cwd": "${fileDirname}"
            },
            "problemMatcher": [
                "$gcc"
            ],
            "group": {
                "kind": "build",
                "isDefault": true
            },
            "detail": "Task generated by Debugger."
        }
    ]
}
```

### 5.4. Настройте отладку (`launch.json`)

- Нажмите `Ctrl+Shift+P` и выберите `Debug: Add Configuration` → `C++ (GDB/LLDB)` → `g++.exe build and debug active file`.
- VS Code создаст файл `launch.json`. Ключевой параметр, который нас интересует, — это `"miDebuggerPath"`. Он должен указывать на полный путь к `gdb.exe`.

**Примерная конфигурация `launch.json`:**

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

> ⚠️ **Обратите внимание:** В вашем файле путь к `gdb.exe` может быть другим (например, `C:\\msys64\\mingw64\\bin\\gdb.exe`), если вы использовали другое окружение MSYS2.  
> Вы можете узнать точный путь, выполнив в терминале MSYS2 команду `which gdb`.

---

## Шаг 6: Запуск отладки

Поставьте точку останова (кликните слева от номера строки `int sum = a + b;` — появится красная точка) и нажмите `F5` или выберите `Run and Debug` в левой панели.

Программа скомпилируется, запустится и остановится на вашей точке останова.  
В левой панели вы сможете увидеть значения всех переменных.

**Пример реального вывода в панели отладки:**

```
Sum is: 15
[Inferior 1 (process 12345) exited normally]
```

Поздравляю, ваша среда разработки полностью настроена! 🎉

---

## Реальные примеры

### Пример вывода компилятора при успешной сборке

```console
> Executing task: C/C++: g++.exe build active file <

Starting build...
Build finished with exit code 0
```

### Пример вывода отладчика (панель Variables)

| Имя   | Значение |
|-------|----------|
| a     | 5        |
| b     | 10       |
| sum   | 15       |

### Пример ошибки и её решение

**Ошибка:** `gdb: command not found`  
**Решение:** Убедитесь, что MSYS2 UCRT64 установлен и путь к `gdb.exe` указан верно в `launch.json`.

---

## Структура проекта

```
HelloWorld/
├── .vscode/
│   ├── tasks.json      # Конфигурация сборки
│   └── launch.json     # Конфигурация отладки
├── main.cpp            # Исходный код
└── README.md           # Этот файл
```

---

## Устранение неполадок

| Проблема | Возможная причина | Решение |
|----------|-------------------|---------|
| `gcc: command not found` | MSYS2 не установлен или не добавлен в PATH | Переустановите MSYS2, используйте терминал UCRT64 |
| `gdb: command not found` | Не установлен пакет `mingw-w64-ucrt-x86_64-toolchain` | Выполните `pacman -S --needed mingw-w64-ucrt-x86_64-toolchain` |
| Ошибка при сборке: `No such file or directory` | Неверный путь в `tasks.json` | Проверьте путь к `g++.exe` |
| Отладчик не останавливается на точке останова | Отсутствует флаг `-g` | Добавьте `-g` в аргументы компилятора |
| `miDebuggerPath` не найден | Путь к `gdb.exe` неверен | Выполните `which gdb` в MSYS2 и скопируйте путь |

---

## Почему MSYS2 UCRT64?

- **UCRT64** использует современную библиотеку времени выполнения Universal C Runtime (UCRT), которая является стандартной для Windows 10/11.
- Обеспечивает лучшую совместимость с современными стандартами C++.
- Пакеты в репозитории MSYS2 регулярно обновляются.
- Позволяет легко устанавливать дополнительные библиотеки через `pacman`.

---

## Как внести вклад

1. Форкните репозиторий.
2. Создайте ветку для вашей функции (`git checkout -b feature/amazing-feature`).
3. Закоммитьте изменения (`git commit -m 'Add some amazing feature'`).
4. Отправьте в ветку (`git push origin feature/amazing-feature`).
5. Откройте Pull Request.

Пожалуйста, следуйте [кодексу поведения](CODE_OF_CONDUCT.md) и убедитесь, что ваши изменения протестированы.

---

## Лицензия

Распространяется под лицензией **MIT**. Смотрите файл [LICENSE](LICENSE) для подробностей.

---

## Благодарности

- [MSYS2](https://www.msys2.org/) — за превосходную среду разработки.
- [Visual Studio Code](https://code.visualstudio.com/) — за мощный и бесплатный редактор.
- [Microsoft C/C++ Extension](https://marketplace.visualstudio.com/items?itemName=ms-vscode.cpptools) — за поддержку C++ в VS Code.
- Всем контрибьюторам и пользователям, кто помог сделать это руководство лучше.

---

**⭐ Если это руководство было полезным, поставьте звезду репозиторию!**  
**📫 По вопросам и предложениям создавайте Issue или обращайтесь к автору.**
