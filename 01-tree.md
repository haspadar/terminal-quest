# 01. `tree`: смотрим на дерево файлов

Перед вами папка `game_server_intro` с данными игрового сервера: логи, профили игроков, матчи, конфиги, чат и ещё один ZIP-архив рядом. Нужно быстро понять, что это за набор и как он устроен.

Перейдите в неё:

```bash
cd game_server_intro
```

Первая естественная команда здесь, конечно, `ls`:

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

Видно, что в папке есть и вложенные папки, и обычные файлы, и архив. Но `ls` показывает только верхний уровень. А что лежит внутри `players`, `logs` или `matches`?

Если нужен более технический взгляд, обычно сразу запускают `ls -la`: он показывает подробности и не прячет скрытые файлы.

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

Попроси `ls` пройтись по всем вложенным папкам:

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

Если объединить рекурсивный обход с подробным выводом и скрытыми файлами, получится уже совсем серьёзная простыня:

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

Это уже лучше, и информации здесь много. Но у такого подхода есть предел. `ls` всё равно показывает содержимое блоками — при большой вложенности глазами читать его тяжело.

Здесь и нужен `tree`. Он решает ту же задачу обзора, но показывает папку в виде дерева. Один запуск, и уже видно, как всё связано между собой.

## Установка

```bash
sudo apt install tree
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

Иногда полное дерево слишком длинное, и на первом проходе такая детализация только мешает. Флаг `-L` ограничивает глубину и помогает сначала посмотреть крупную структуру.

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

Здесь главное, что `players` уже виден, но его содержимое обрезано до уровня регионов. Для первого обзора этого обычно достаточно.

Набери `tree -L 2` и посмотри, что видно на верхнем уровне: `chat`, `configs`, `items`, `logs`, `matches`, `players`, `page.html`, `ops_snapshot_2026-02-22.zip`.

### Показать только директории: `-d`

Если тебя интересуют не файлы, а папки, лишний шум лучше убрать. Флаг `-d` оставляет только папки.

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

Такой вывод удобен, когда ты сначала хочешь понять карту папок, а не разбираться в файлах внутри них.

Набери `tree -d` и проверь, какие регионы есть внутри `players`. 

Должны быть видны `america`, `asia`, `europe`.

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

По этому фрагменту видно, что в наборе есть ещё и скрытые служебные объекты. Без `-a` ты бы их просто не заметил.

Набери `tree -a` и найди скрытые служебные объекты.

Должны быть видны `.cache`, `.env.example`, `.notes`, `.drafts`.

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

Это полезно, когда путь потом нужно вставить в другую команду или просто не хочется восстанавливать его в голове.

### Оставить только подходящие имена: `-P`

Иногда не нужно смотреть всё дерево целиком, а хочется быстро вытащить только объекты с определённым именем. Флаг `-P` задаёт шаблон отбора, а `--prune` убирает пустые ветки, которые после отбора больше не нужны.

```bash
tree --prune -P '*anna*'
```

Фрагмент вывода:

```text
.
├── chat
├── configs
├── items
│   └── items/anna_inventory.csv
├── logs
├── matches
└── players
    ├── america
    ├── asia
    └── europe
        └── players/europe/anna.txt

10 directories, 2 files
```

Обрати внимание: `tree` оставил не только сами совпавшие файлы, но и папки, через которые к ним можно дойти. Это нормально: иначе путь до найденного файла просто потерялся бы.

Если добавить `-f`, `tree` просто начнёт печатать полные пути в выводе. Саму фильтрацию делает не `-f`, а связка `-P` и `--prune`.

Набери `tree --prune -P '*anna*'` и посмотри, что осталось в дереве.

Должны остаться `items/anna_inventory.csv` и `players/europe/anna.txt`.

### Исключить часть файлов: `-I`

Бывает и обратная задача: структура нужна целиком, но какой-то тип файлов только захламляет картину. Флаг `-I` скрывает имена, которые подходят под шаблон исключения.

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

Здесь хорошо видно разницу между “скрыть файлы” и “скрыть папку”: папка `logs` осталась, но её содержимое больше не отвлекает.

Набери `tree -L 2 -I '*.log'` и посмотри, что стало с `logs`.

Должна остаться папка `logs`, но файлы `2026-02-20.log`, `2026-02-21.log`, `2026-02-22.log` показываться не должны.

### Права, владелец и группа: `-pug`

Иногда важно не только что лежит в папке, но и кто этим владеет и какие там права доступа. Флаги `-pug` добавляют эту служебную информацию рядом с именами.

```bash
tree -pug configs
```

Фрагмент вывода:

```text
[drwxr-xr-x USER GROUP ]  configs
├── [-rw-r--r-- USER GROUP ]  maps.json
└── [-rw-r--r-- USER GROUP ]  server.ini

1 directory, 2 files
```

Имена пользователя и группы зависят от системы.

Такой режим нужен реже, но он полезен, если ты проверяешь доступ к конфигам или чужим рабочим папкам.

### Размеры: `-s`, `-h`, `--du`

Если папка кажется подозрительно тяжёлой, полезно сразу увидеть, где лежит основной объём. Эти флаги показывают размеры файлов и суммарный размер папки в более читаемом виде.

```bash
tree -h --du
```

Фрагмент вывода:

```text
[ 13K]  .
├── [ 531]  chat
│   ├── [ 124]  clan_dragons.log
│   └── [ 279]  global_chat.log
├── [ 638]  configs
│   ├── [ 337]  maps.json
│   └── [ 173]  server.ini
├── [1.0K]  items
...
├── [6.0K]  logs
...
├── [1.3K]  ops_snapshot_2026-02-22.zip
├── [ 539]  page.html
└── [1.4K]  players
...

13K used in 10 directories, 27 files
```

В этом режиме `logs` сразу бросается в глаза как самая тяжёлая папка. Именно для таких быстрых оценок размеры и показывают.

### Дата изменения: `-D`

Когда нужно понять, какие файлы обновлялись недавно, полезно видеть время изменения прямо в дереве. Флаг `-D` добавляет дату к каждому объекту.

```bash
tree -D configs
```

Фрагмент вывода:

```text
[DATE]  configs
├── [Feb 28 19:00]  maps.json
└── [Feb 28 19:00]  server.ini

1 directory, 2 files
```

Дата самой папки зависит от распаковки. А даты файлов сохраняются из архива.

Этот флаг удобен, когда нужно быстро понять, что менялось недавно, не заходя в `ls -l` по каждой папке отдельно.

### Сначала директории: `--dirsfirst`

Обычно удобнее сначала увидеть папки, а уже потом отдельные файлы на том же уровне. Опция `--dirsfirst` просто меняет порядок вывода именно в эту сторону.

```bash
tree --dirsfirst -L 2
```

На верхнем уровне сначала идут папки `chat`, `configs`, `items`, `logs`, `matches`, `players`, а уже потом файлы `ops_snapshot_2026-02-22.zip` и `page.html`.

Мелочь, но читать такой вывод обычно проще: сначала структура, потом одиночные файлы.

### Записать вывод в файл: `-o`

Иногда дерево нужно не просто посмотреть, а сохранить: например, чтобы отправить, сравнить позже или приложить к заметкам. Опция `-o` записывает вывод в файл вместо экрана.

```bash
tree -L 2 -o structure.txt
```

После этого дерево будет записано в `structure.txt`.

Это удобно для заметок, отчёта или просто чтобы потом сравнить структуру папки до и после изменений.

Набери `tree -L 2 -o structure.txt` и посмотри, что появился новый файл.

Рядом должен появиться `structure.txt` с сохранённым выводом.

## Где граница у `tree`

Стандартный `tree` хорошо показывает структуру папки и метаданные файлов. Но это именно просмотрщик дерева, а не файловый менеджер: он не перемещает файлы, не редактирует их и не умеет сам по себе показывать содержимое ZIP-архивов.

Если тебе попалась статья, где команда с похожим названием умеет больше, проверь имя утилиты внимательно. Например, в документации RED OS есть материал про `tre-command`: это другой инструмент, не стандартный `tree`.

## Как быстро заглянуть в ZIP-архив

`tree` хорошо показывает, что архив лежит рядом с остальными данными, но внутрь ZIP-файла он не заходит. Если нужно посмотреть, что лежит внутри архива, не распаковывая его, используйте `unzip -l`.

Здесь `-l` означает list, то есть “показать список файлов внутри архива”. Команда читает содержимое архива, но не создаёт файлы и папки на диске.

Сначала можно просто убедиться, что архив вообще лежит рядом с остальными объектами:

```bash
tree -L 1
```

Фрагмент вывода:

```text
.
├── chat
├── configs
├── items
├── logs
├── matches
├── ops_snapshot_2026-02-22.zip
├── page.html
└── players
```

Теперь можно отдельно заглянуть в сам архив:

```bash
unzip -l ops_snapshot_2026-02-22.zip
```

Вывод:

```text
Archive:  ops_snapshot_2026-02-22.zip
  Length      Date    Time    Name
---------  ---------- -----   ----
      173  02-28-2026 19:00   configs/server.ini
     1975  02-28-2026 19:00   logs/2026-02-22.log
      115  02-28-2026 19:00   players/europe/anna.txt
---------                     -------
     2263                     3 files
```

Это не дерево, но для быстрой разведки работает отлично. А если нужно не просто посмотреть список, а сразу отфильтровать его, можно подключить уже знакомый `grep`:

```bash
unzip -l ops_snapshot_2026-02-22.zip | grep anna
```

Здесь `grep` ищет нужное имя уже не по папке на диске, а по списку файлов внутри архива.

## Шпаргалка

- `tree` показать дерево текущей папки.
- `ls -la` показать подробный список, включая скрытые файлы.
- `ls -Rla` рекурсивно показать содержимое папки с подробностями и скрытыми файлами.
- `tree путь` показать дерево указанной папки.
- `tree -L 2` ограничить глубину.
- `tree -d` показать только папки.
- `tree -a` показать скрытые файлы.
- `tree -f` показать полный путь.
- `tree --prune -P 'шаблон'` оставить только подходящие имена и убрать пустые ветки.
- `tree -I 'шаблон'` скрыть подходящие имена.
- `tree -pug` показать права, владельца и группу.
- `tree -h --du` показать размеры.
- `tree -D` показать дату изменения.
- `tree -o файл` записать вывод в файл.
- `unzip -l архив.zip` посмотреть список файлов внутри ZIP без распаковки.

## Напоследок

1. `tree` появился не в Linux.
Сначала была DOS-команда `TREE`, а уже примерно через 8 лет появилась Unix/Linux-версия этой утилиты.

2. `tree` не входит в GNU coreutils.
Поэтому на Ubuntu его обычно нужно ставить отдельно, например через `sudo apt install tree`.

3. `tree` до сих пор развивается.
У современных версий есть вывод не только в терминал, но и в JSON, HTML и XML.

Набери `tree -J -L 2`.

Должен появиться JSON с описанием верхней части дерева.

## Практика

1.1. Покажи дерево с размерами и найди, какая папка занимает больше всего места.

   Должна выделяться папка `logs`.

1.2. Запиши дерево до глубины 2 в файл `structure.txt`.

   Должен появиться файл `structure.txt` с сохранённым выводом дерева.

1.3. Покажи дерево с датами для `configs`.

   У `maps.json` и `server.ini` должна быть показана дата `Feb 28 19:00`.

1.4. Покажи дерево без лог-файлов: папка `logs` должна остаться, но файлы внутри неё показываться не должны.

   Папка `logs` должна остаться в дереве, но файлы `2026-02-20.log`, `2026-02-21.log`, `2026-02-22.log` показываться не должны.
