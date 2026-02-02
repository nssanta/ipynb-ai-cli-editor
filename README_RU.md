# Редактор Jupyter Notebook для AI-агентов

[![Python 3.6+](https://img.shields.io/badge/python-3.6+-blue.svg)](https://www.python.org/downloads/)
[![Без зависимостей](https://img.shields.io/badge/зависимости-ноль-green.svg)](https://github.com/yourusername/notebook-editor)
[![Лицензия: MIT](https://img.shields.io/badge/Лицензия-MIT-yellow.svg)](https://opensource.org/licenses/MIT)

**Инструмент командной строки** для программного редактирования файлов Jupyter Notebook (`.ipynb`) **без внешних зависимостей**. Специально разработан для работы с большими языковыми моделями (LLM) и автоматизированными рабочими процессами.

[English Version](README.md) | [Русская версия](README_RU.md)

---

## 🎯 Зачем этот инструмент?

Традиционное редактирование Jupyter notebook требует:

- Полноценного окружения Jupyter с тяжёлыми зависимостями
- Ручного редактирования JSON (подвержено ошибкам и хрупко)
- Сложных библиотек, которые могут не работать в ограниченных средах

**Этот инструмент решает эти проблемы**, предоставляя:

✅ **Ноль зависимостей** - Использует только стандартную библиотеку Python  
✅ **Оптимизирован для AI-агентов** - Чистый CLI интерфейс с паттерном обмена файлами  
✅ **Безопасен для JSON** - Сохраняет структуру и метаданные ноутбука  
✅ **Портативный** - Работает везде, где установлен Python 3.6+  
✅ **Надёжный** - Предсказуемое поведение для автоматизированных процессов  

---

## 🚀 Быстрый старт

### Установка

Установка не требуется! Просто скачайте скрипт:

```bash
wget https://raw.githubusercontent.com/yourusername/notebook-editor/main/notebook_editor.py
chmod +x notebook_editor.py
```

Или клонируйте репозиторий:

```bash
git clone https://github.com/yourusername/notebook-editor.git
cd notebook-editor
```

### Базовое использование

```bash
# Список всех ячеек в ноутбуке
python3 notebook_editor.py list my_notebook.ipynb

# Прочитать конкретную ячейку с номерами строк
python3 notebook_editor.py read my_notebook.ipynb 5 --numbered

# Обновить ячейку из файла
python3 notebook_editor.py update my_notebook.ipynb 5 --from-file modified_content.py

# Редактировать только конкретные строки
python3 notebook_editor.py patch my_notebook.ipynb 5 --lines 10-15 --from-file patch.py

# Очистить выводы всех ячеек
python3 notebook_editor.py clear-output my_notebook.ipynb --all

# Поиск содержимого
python3 notebook_editor.py search my_notebook.ipynb "import pandas"
```

---

## 📖 Полный справочник команд

### 1. **list** - Просмотр структуры ноутбука

Показывает все ячейки с их индексами, типами и превью содержимого.

```bash
python3 notebook_editor.py list <notebook.ipynb> [--limit N] [--json]
```

**Примеры:**

```bash
# Обычный вывод
python3 notebook_editor.py list analysis.ipynb --limit 20

# JSON вывод (для парсинга LLM)
python3 notebook_editor.py list analysis.ipynb --json
```

**JSON вывод:**

```json
{
  "notebook": "analysis.ipynb",
  "total_cells": 15,
  "cells": [
    {"index": 0, "type": "code", "lines": 10, "has_output": true, ...}
  ]
}
```

---

### 2. **read** - Извлечение содержимого ячейки

Читает содержимое конкретной ячейки в консоль или файл.

```bash
python3 notebook_editor.py read <notebook.ipynb> <индекс> [--to-file <файл>] [--numbered] [--include-output]
```

**Примеры:**

```bash
# Вывод с номерами строк — удобно для patch
python3 notebook_editor.py read analysis.ipynb 5 --numbered

# Сохранение в файл
python3 notebook_editor.py read analysis.ipynb 5 --to-file cell_5.py

# Вместе с результатами выполнения
python3 notebook_editor.py read analysis.ipynb 5 --include-output
```

**Вывод с --numbered:**

```
--- Cell 5 (code) [17 lines] ---
 1: import pandas as pd
 2: import numpy as np
 3: 
 4: def calculate_mean(data):
 5:     """Calculate the mean."""
 6:     return sum(data) / len(data)
...
```

---

### 3. **search** - Поиск содержимого

Поиск текста или regex-паттернов во всех ячейках.

```bash
python3 notebook_editor.py search <notebook.ipynb> "<запрос>" [--regex]
```

**Примеры:**

```bash
# Простой текстовый поиск
python3 notebook_editor.py search analysis.ipynb "import pandas"

# Поиск по регулярному выражению
python3 notebook_editor.py search analysis.ipynb "def .*_handler" --regex
```

---

### 4. **update** - Изменение содержимого ячейки

Заменяет содержимое существующей ячейки целиком.

```bash
python3 notebook_editor.py update <notebook.ipynb> <индекс> --from-file <файл>
python3 notebook_editor.py update <notebook.ipynb> <индекс> --content "<текст>"
```

**Примеры:**

```bash
# Обновление из файла (РЕКОМЕНДУЕТСЯ)
python3 notebook_editor.py update analysis.ipynb 5 --from-file modified_code.py

# Сохранить выходные данные (не очищать)
python3 notebook_editor.py update analysis.ipynb 5 --from-file code.py --no-clear-output
```

---

### 5. **patch** - Редактирование конкретных строк

Заменяет только указанные строки в ячейке. **Гораздо эффективнее чем update!**

```bash
python3 notebook_editor.py patch <notebook.ipynb> <индекс> --lines <диапазон> --from-file <файл>
python3 notebook_editor.py patch <notebook.ipynb> <индекс> --lines <диапазон> --content "<текст>"
```

**Примеры:**

```bash
# Заменить строки 5-10
python3 notebook_editor.py patch analysis.ipynb 3 --lines 5-10 --from-file patch.py

# Заменить одну строку
python3 notebook_editor.py patch analysis.ipynb 3 --lines 7-7 --content "new_value = 42"

# Вставить после строки 5 (не заменять, а добавить)
python3 notebook_editor.py patch analysis.ipynb 3 --lines 5 --insert --from-file insert.py

# Без автоматического сохранения отступов
python3 notebook_editor.py patch analysis.ipynb 3 --lines 5-10 --from-file patch.py --no-preserve-indent
```

**Ключевые особенности:**
- ✅ Автоматически сохраняет относительные отступы
- ✅ Режим вставки (`--insert`) — добавляет код без замены
- ✅ Автоочистка выводов после редактирования

---

### 6. **add** - Вставка новой ячейки

Добавляет новую code или markdown ячейку.

```bash
python3 notebook_editor.py add <notebook.ipynb> --type <code|markdown> --from-file <файл>
```

**Примеры:**

```bash
# Добавить в начало
python3 notebook_editor.py add analysis.ipynb --index 0 --type markdown --content "# Введение"

# Добавить в конец (по умолчанию)
python3 notebook_editor.py add analysis.ipynb --type code --from-file new_analysis.py
```

---

### 7. **delete** - Удаление ячейки

```bash
python3 notebook_editor.py delete <notebook.ipynb> <индекс>
```

---

### 8. **diff** - Предпросмотр изменений

Показывает, что изменится перед обновлением ячейки.

```bash
python3 notebook_editor.py diff <notebook.ipynb> <индекс> --from-file <файл>
```

---

### 9. **create** - Создание нового ноутбука

```bash
python3 notebook_editor.py create <notebook.ipynb>
```

---

### 10. **clear-output** - Очистка выводов ячеек

Удаляет результаты выполнения из ячеек.

```bash
python3 notebook_editor.py clear-output <notebook.ipynb> --all
python3 notebook_editor.py clear-output <notebook.ipynb> --cells 0 2 5
```

**Примеры:**

```bash
# Очистить все code-ячейки
python3 notebook_editor.py clear-output analysis.ipynb --all

# Очистить конкретные ячейки
python3 notebook_editor.py clear-output analysis.ipynb --cells 0 2 5
```

---

### 11. **info** - Метаданные ноутбука

Показывает информацию о ноутбуке.

```bash
python3 notebook_editor.py info <notebook.ipynb>
```

**Вывод:**

```
Notebook: analysis.ipynb
Format: nbformat 4.5
Kernel: Python 3
Cells: 25 total
  - Code: 18
  - Markdown: 7
  - With outputs: 12
Total source lines: 450
```

---

### 12. **validate** - Проверка структуры

Проверяет валидность JSON-структуры ноутбука.

```bash
python3 notebook_editor.py validate <notebook.ipynb>
```

Возвращает exit code 1 при ошибках — удобно для CI/CD.

---

### 13. **save-output** - Извлечение изображений

```bash
python3 notebook_editor.py save-output <notebook.ipynb> <индекс> --to-file <путь>
```

---

### 14. **export** - Экспорт в Markdown или Python
322: 
323: Экспортирует весь ноутбук в один Markdown (`.md`) или Python (`.py`) файл, включая вывод ячеек и изображения.
324: 
325: - Для `.py`: Markdown-ячейки и выводы сохраняются как `#` комментарии.
326: - Для `.md`: Markdown-ячейки и выводы сохраняются как `<!-- -->` комментарии.
327: - **Картинки**: Автоматически извлекаются и сохраняются в папку. Если папка уже существует, добавляется суффикс `_copyN`.
328: 
329: ```bash
330: python3 notebook_editor.py export <notebook.ipynb> <output.md|.py> [--image-dir <папка>]
331: ```
332: 
333: **Примеры:**
334: 
335: ```bash
336: # Экспорт в python (готов к запуску, с комментариями)
337: python3 notebook_editor.py export analysis.ipynb script.py
338: 
339: # Экспорт в markdown
340: python3 notebook_editor.py export analysis.ipynb report.md
341: ```
338: 
339: ---
340: 
341: ## 📋 Сводная таблица команд

| Команда | Описание | Ключевые флаги |
|---------|----------|----------------|
| `list` | Просмотр структуры | `--limit`, `--json` |
| `read` | Чтение ячейки | `--numbered`, `--to-file`, `--include-output` |
| `search` | Поиск текста | `--regex` |
| `update` | Замена всей ячейки | `--from-file`, `--no-clear-output` |
| `patch` | Редактирование строк | `--lines`, `--insert`, `--no-preserve-indent` |
| `add` | Добавление ячейки | `--index`, `--type`, `--from-file` |
| `delete` | Удаление ячейки | - |
| `diff` | Предпросмотр изменений | `--from-file` |
| `clear-output` | Очистка выводов | `--all`, `--cells` |
| `info` | Метаданные | - |
| `validate` | Проверка структуры | - |
| `create` | Новый ноутбук | - |
| `save-output` | Извлечение картинок | `--output-index`, `--to-file` |
| `export` | Экспорт в Markdown | `--image-dir` |

---

## 🤖 Лучшие практики для AI-агентов

> **Примечание для пользователей:** Специальное руководство для AI-агентов находится в файле [`README_AGENT.md`](README_AGENT.md).

### Рекомендуемый рабочий процесс

```bash
# 1. Исследование: Понять структуру
python3 notebook_editor.py list notebook.ipynb

# 2. Чтение с номерами строк
python3 notebook_editor.py read notebook.ipynb 5 --numbered

# 3. Точечное редактирование (эффективнее чем update)
python3 notebook_editor.py patch notebook.ipynb 5 --lines 10-15 --from-file patch.py

# ИЛИ полная замена ячейки
python3 notebook_editor.py read notebook.ipynb 5 --to-file temp.py
# (редактирование temp.py)
python3 notebook_editor.py update notebook.ipynb 5 --from-file temp.py
```

### Преимущества patch перед update

| update | patch |
|--------|-------|
| Нужно копировать всю ячейку | Редактируем только нужные строки |
| Легко ошибиться в отступах | Отступы сохраняются автоматически |
| Много токенов | Экономим токены |

---

## 📋 Требования

- **Python 3.6+** (внешние пакеты не требуются)
- Работает на Linux, macOS и Windows

---

## 🔧 Технические детали

### Формат файла

Инструмент работает со стандартным форматом Jupyter Notebook (`.ipynb`), который основан на JSON:

- Сохраняет все метаданные
- Поддерживает счётчики выполнения ячеек
- Обрабатывает как code, так и markdown ячейки
- Поддерживает многострочное содержимое с правильным форматированием

### Функции безопасности

- Валидирует структуру JSON перед сохранением
- Создаёт резервные копии неявно (используйте систему контроля версий!)
- Обрабатывает граничные случаи (пустые ячейки, специальные символы и т.д.)
- Предоставляет понятные сообщения об ошибках

---

## 📝 Примеры

### Пример 1: Быстрое исправление одной функции

```bash
# Смотрим ячейку с номерами строк
python3 notebook_editor.py read notebook.ipynb 3 --numbered

# Заменяем только строки 5-8
python3 notebook_editor.py patch notebook.ipynb 3 --lines 5-8 --content "    return x * 2"
```

### Пример 2: Добавление валидации в функцию

```bash
# Вставляем новый код после строки 4
echo "    if x is None:
        raise ValueError('x cannot be None')" > insert.py

python3 notebook_editor.py patch notebook.ipynb 3 --lines 4 --insert --from-file insert.py
```

### Пример 3: Очистка перед коммитом

```bash
# Очищаем все выводы
python3 notebook_editor.py clear-output notebook.ipynb --all

# Проверяем валидность
python3 notebook_editor.py validate notebook.ipynb
```

---

## 📄 Лицензия

Лицензия MIT - свободно используйте в своих проектах!

---

**Сделано с ❤️ для AI-агентов и разработчиков**
