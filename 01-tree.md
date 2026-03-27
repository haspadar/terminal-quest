# 01. `tree`: смотрим на дерево файлов

Папка `game_server_intro` — данные игрового сервера: логи, профили игроков, матчи, конфиги, чат, ZIP-архив. Хочется быстро понять, что тут вообще есть.

Перейдите в неё:

```bash
cd game_server_intro
```

Стандартная команда для начала — `ls`:

```bash
ls
```

Вывод:

```text
chat
configs
items
logs
matches
ops_snapshot_2026-02-22.zip
page.html
players
```

`ls` показывает только первый уровень. Что внутри `players`, `logs` или `matches` — непонятно.

Чтобы увидеть больше деталей, используйте `ls -la`: он показывает все атрибуты и скрытые файлы.

```bash
ls -la
```

Вывод:

```text
total 44
drwxr-xr-x 10 student student 4096 Feb 22 12:00 .
drwxr-xr-x  3 student student 4096 Feb 22 12:00 ..
drwxr-xr-x  2 student student 4096 Feb 22 12:00 .cache
-rw-r--r--  1 student student   42 Feb 22 12:00 .env.example
-rw-r--r--  1 student student   67 Feb 22 12:00 .notes
drwxr-xr-x  2 student student 4096 Feb 22 12:00 chat
drwxr-xr-x  2 student student 4096 Feb 22 12:00 configs
drwxr-xr-x  2 student student 4096 Feb 22 12:00 items
drwxr-xr-x  2 student student 4096 Feb 22 12:00 logs
drwxr-xr-x  2 student student 4096 Feb 22 12:00 matches
-rw-r--r--  1 student student 1820 Feb 22 12:00 ops_snapshot_2026-02-22.zip
-rw-r--r--  1 student student  812 Feb 22 12:00 page.html
drwxr-xr-x  6 student student 4096 Feb 22 12:00 players
```

Теперь видно, что в наборе есть скрытые служебные файлы и папки. Но содержимое вложенных директорий всё ещё не раскрыто.

Флаг `-R` обходит все вложенные папки:

```bash
ls -R
```

Фрагмент вывода:

```text
chat
configs
items
logs
matches
ops_snapshot_2026-02-22.zip
page.html
players

./chat:
clan_dragons.log
global_chat.log

./configs:
maps.json
server.ini

./players:
america
asia
europe
```

Можно объединить оба флага — вывод будет длинным:

```bash
ls -Rla
```

Фрагмент вывода:

```text
.:
total 44
drwxr-xr-x 10 student student 4096 Feb 22 12:00 .
drwxr-xr-x  3 student student 4096 Feb 22 12:00 ..
drwxr-xr-x  2 student student 4096 Feb 22 12:00 .cache
-rw-r--r--  1 student student   42 Feb 22 12:00 .env.example
-rw-r--r--  1 student student   67 Feb 22 12:00 .notes
drwxr-xr-x  6 student student 4096 Feb 22 12:00 players

./players:
total 24
drwxr-xr-x  6 student student 4096 Feb 22 12:00 .
drwxr-xr-x 10 student student 4096 Feb 22 12:00 ..
drwxr-xr-x  2 student student 4096 Feb 22 12:00 .drafts
drwxr-xr-x  2 student student 4096 Feb 22 12:00 america
drwxr-xr-x  2 student student 4096 Feb 22 12:00 asia
drwxr-xr-x  2 student student 4096 Feb 22 12:00 europe

./players/.drafts:
total 8
drwxr-xr-x 2 student student 4096 Feb 22 12:00 .
drwxr-xr-x 6 student student 4096 Feb 22 12:00 ..
-rw-r--r-- 1 student student   19 Feb 22 12:00 anna.tmp
```

Но `ls` выводит содержимое блоками: при глубокой вложенности это тяжело читать.

`tree` решает ту же задачу, но выводит структуру деревом — сразу видно, что где лежит.

## Установка

Ubuntu / WSL:

```bash
sudo apt install tree
```

Arch:

```bash
sudo pacman -S tree
```

## Синтаксис

```bash
tree [опции] [путь]
```

Если путь не указан, `tree` работает в текущей папке.

## Базовый запуск

```bash
tree
```

Вывод:

```text
.
├── chat
│   ├── clan_dragons.log
│   └── global_chat.log
├── configs
│   ├── maps.json
│   └── server.ini
├── items
│   ├── anna_inventory.csv
│   ├── carlos_inventory.csv
│   ├── ivan_inventory.csv
│   ├── john_inventory.csv
│   ├── kenji_inventory.csv
│   ├── li_inventory.csv
│   └── max_inventory.csv
├── logs
│   ├── 2026-02-20.log
│   ├── 2026-02-21.log
│   └── 2026-02-22.log
├── matches
│   ├── match_1001.txt
│   ├── match_1002.txt
│   ├── match_1003.txt
│   └── match_1004.txt
├── ops_snapshot_2026-02-22.zip
├── page.html
└── players
    ├── america
    │   ├── carlos.txt
    │   └── john.txt
    ├── asia
    │   ├── kenji.txt
    │   └── li.txt
    └── europe
        ├── anna.txt
        ├── ivan.txt
        └── max.txt

10 directories, 27 files
```

Посмотрим в другую папку:

```bash
tree players
```

## Основные опции

### Ограничить глубину: `-L`

Полное дерево бывает длинным. Флаг `-L` ограничивает глубину вывода.

```bash
tree -L 2
```

Вывод:

```text
.
├── chat
│   ├── clan_dragons.log
│   └── global_chat.log
├── configs
│   ├── maps.json
│   └── server.ini
├── items
│   ├── anna_inventory.csv
│   ├── carlos_inventory.csv
│   ├── ivan_inventory.csv
│   ├── john_inventory.csv
│   ├── kenji_inventory.csv
│   ├── li_inventory.csv
│   └── max_inventory.csv
├── logs
│   ├── 2026-02-20.log
│   ├── 2026-02-21.log
│   └── 2026-02-22.log
├── matches
│   ├── match_1001.txt
│   ├── match_1002.txt
│   ├── match_1003.txt
│   └── match_1004.txt
├── ops_snapshot_2026-02-22.zip
├── page.html
└── players
    ├── america
    ├── asia
    └── europe

10 directories, 20 files
```

`players` виден, но раскрыт только на один уровень — видны регионы, не файлы.

### Показать только директории: `-d`

Если нужны только папки, используйте флаг `-d` — он скрывает файлы.

```bash
tree -d
```

Вывод:

```text
.
├── chat
├── configs
├── items
├── logs
├── matches
└── players
    ├── america
    ├── asia
    └── europe

10 directories
```

Удобно, когда нужно сначала разобраться в структуре папок, не отвлекаясь на файлы.

### Показать скрытые файлы: `-a`

По умолчанию `tree` не показывает скрытые файлы и папки, хотя именно там часто лежат служебные данные. Флаг `-a` включает их в вывод.

```bash
tree -a
```

Фрагмент вывода:

```text
.
├── .cache
│   └── index.txt
├── .env.example
├── .notes
├── chat
├── configs
├── items
├── logs
├── matches
├── ops_snapshot_2026-02-22.zip
├── page.html
└── players
    ├── .drafts
    │   └── anna.tmp
    ├── america
    ├── asia
    └── europe

12 directories, 31 files
```

По этому фрагменту видно, что в наборе есть ещё и скрытые служебные объекты. Без `-a` вы бы их просто не заметили.

### Показать полный путь: `-f`

Обычное дерево удобно читать глазами, но иногда нужен не просто список имён, а полный путь к каждому файлу. Флаг `-f` добавляет его прямо в вывод.

```bash
tree -f players
```

Вывод:

```text
players
├── players/america
│   ├── players/america/carlos.txt
│   └── players/america/john.txt
├── players/asia
│   ├── players/asia/kenji.txt
│   └── players/asia/li.txt
└── players/europe
    ├── players/europe/anna.txt
    ├── players/europe/ivan.txt
    └── players/europe/max.txt

4 directories, 7 files
```

Это полезно, когда путь нужно использовать в другой команде.

### Оставить только подходящие имена: `-P`

Иногда нужно найти только файлы с определённым именем, не смотря на всё дерево. Флаг `-P` задаёт шаблон отбора, а `--prune` убирает пустые ветки, которые после отбора больше не нужны.

```bash
tree --prune -P '*anna*'
```

Фрагмент вывода:

```text
.
├── items
│   └── items/anna_inventory.csv
└── players
    └── europe
        └── players/europe/anna.txt

4 directories, 2 files
```

Обратите внимание: `tree` оставил не только сами совпавшие файлы, но и папки, через которые к ним можно дойти.

### Исключить часть файлов: `-I`

Обратная задача: нужна структура целиком, но какой-то тип файлов мешает. Флаг `-I` исключает имена по шаблону.

```bash
tree -L 2 -I '*.log'
```

Вывод:

```text
.
├── chat
├── configs
│   ├── maps.json
│   └── server.ini
├── items
│   ├── anna_inventory.csv
│   ├── carlos_inventory.csv
│   ├── ivan_inventory.csv
│   ├── john_inventory.csv
│   ├── kenji_inventory.csv
│   ├── li_inventory.csv
│   └── max_inventory.csv
├── logs
├── matches
│   ├── match_1001.txt
│   ├── match_1002.txt
│   ├── match_1003.txt
│   └── match_1004.txt
├── ops_snapshot_2026-02-22.zip
├── page.html
└── players
    ├── america
    ├── asia
    └── europe

10 directories, 15 files
```

Видна разница: папка `logs` осталась, но её содержимое скрыто.

### Сначала директории: `--dirsfirst`

Обычно удобнее сначала увидеть папки, а уже потом отдельные файлы на том же уровне. Опция `--dirsfirst` меняет порядок: сначала папки, потом файлы.

```bash
tree --dirsfirst -L 2
```

На верхнем уровне сначала идут папки — `chat`, `configs`, `items`, `logs`, `matches`, `players`, — потом файлы `ops_snapshot_2026-02-22.zip` и `page.html`.

### Записать вывод в файл: `-o`

Иногда дерево нужно не просто посмотреть, а сохранить: например, чтобы отправить, сравнить позже или приложить к заметкам. Опция `-o` записывает вывод в файл вместо экрана.

```bash
tree -L 2 -o structure.txt
```

Дерево запишется в `structure.txt` — удобно для заметок, отчёта или сравнения структуры до и после изменений.

## Шпаргалка

- `tree <путь>` показать дерево указанной папки.
- `tree -L <глубина>` ограничить глубину.
- `tree -d` показать только папки.
- `tree -a` показать скрытые файлы.
- `tree -f` показать полный путь.
- `tree --prune -P 'шаблон'` оставить только подходящие имена и убрать пустые ветки.
- `tree -I 'шаблон'` скрыть подходящие имена.
- `tree -o файл` записать вывод в файл.

## Напоследок

1. Утилита `tree` пришла из мира DOS. Первая версия для UNIX/Linux появилась в 1990-х годах.

2. `tree` не входит в GNU coreutils, поэтому его обычно ставят отдельно: на Ubuntu через `sudo apt install tree`, на Arch через `sudo pacman -S tree`.

## Практика

1.1. Выведите дерево файлов с именем, содержащим `anna`, но без расширения `.tmp`. Глубина — 3, без пустых веток, результат сохраните в `/tmp/anna-tree.txt`.

Ожидаемый результат:

```text
.
├── items
│   └── items/anna_inventory.csv
└── players
    └── europe
        └── players/europe/anna.txt

4 directories, 2 files
```

Файл `/tmp/anna-tree.txt` должен появиться на диске с этим деревом.
