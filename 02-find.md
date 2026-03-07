# 02. `find`: ищем файлы и папки

После `tree` мы знаем структуру директории. Но как нам найти конкретные файлы? Могут возникнуть такие вопросы:

- где все инвентари;
- где профили игроков из Европы;
- где только папки, а где только файлы;
- как не лазить по дереву глазами каждый раз.

Для этого и нужен `find`.

Перейди в папку:

```bash
cd game_server_intro
```

## Синтаксис

Чтобы не запутаться, удобнее сразу смотреть на `find` как на команду из трёх частей:

- где искать;
- по каким условиям искать;
- что делать с найденным.

Самый общий каркас такой:

```bash
find PATH CONDITIONS ACTION
```

Например:

```bash
find . -type f -name '*.csv'
```

Здесь:
- `.` — где искать;
- `-type f -name '*.csv'` — по каким условиям отбирать;
- отдельного действия нет, поэтому `find` просто выводит результат.

У `find` есть и более полный вариант:

```bash
find PATH CONDITIONS -exec COMMAND {} \;
```

Здесь:
- `PATH` — откуда начинать поиск;
- `CONDITIONS` — по каким правилам отбирать файлы и папки;
- `-exec COMMAND {} \;` — что сделать с каждым найденным результатом.

И это хороший момент, чтобы понять одну важную вещь: `find` задумывали не просто как “поиск файлов”, а как маленький язык запросов.

Поэтому у него есть:
- условия;
- логические операторы;
- скобки;
- действия.

Например:

```bash
find . -type f -name '*.php' -size +1M
```

Это буквально читается так:

найти файлы AND имя `*.php` AND размер больше 1 MB

## Искать по имени: `-name`

Самый частый первый шаг: найти файлы по шаблону имени.

```bash
find . -name '*.csv'
```

Вывод:

```text
./items/anna_inventory.csv
./items/carlos_inventory.csv
./items/ivan_inventory.csv
./items/john_inventory.csv
./items/kenji_inventory.csv
./items/li_inventory.csv
./items/max_inventory.csv
```

Точка `.` означает “искать от текущей папки”. А `*.csv` означает “все имена, которые заканчиваются на `.csv`”.

Набери `find . -name '*.csv'`.

Должны найтись все 7 файлов из `items`.

## Искать по расширению в конкретной папке

Часто тебя интересует не весь набор, а одна папка. Например, только инвентари.

```bash
find items -name '*.csv'
```

Вывод:

```text
items/anna_inventory.csv
items/carlos_inventory.csv
items/ivan_inventory.csv
items/john_inventory.csv
items/kenji_inventory.csv
items/li_inventory.csv
items/max_inventory.csv
```

Здесь `find` уже не бродит по всему набору, а ищет только внутри `items`.

Набери `find items -name '*.csv'`.

Должны найтись все 7 файлов из `items`, но уже без других папок в поиске.

## Искать без учёта регистра: `-iname`

Иногда ты не уверен, как именно записано имя: маленькими буквами или большими. Для такого случая есть `-iname`.

```bash
find . -iname '*anna*'
```

Вывод:

```text
./items/anna_inventory.csv
./players/.drafts/anna.tmp
./players/europe/anna.txt
```

Разница с `-name` такая: `-iname` не различает регистр.

Набери `find . -iname '*anna*'`.

Должны найтись:
- `./items/anna_inventory.csv`
- `./players/.drafts/anna.tmp`
- `./players/europe/anna.txt`

## Отделить файлы от папок: `-type`

Часто одного имени мало. Например, тебя интересуют только файлы или только папки.

```bash
find players -type f
```

Вывод:

```text
players/.drafts/anna.tmp
players/america/carlos.txt
players/america/john.txt
players/asia/kenji.txt
players/asia/li.txt
players/europe/anna.txt
players/europe/ivan.txt
players/europe/max.txt
```

Флаг `-type f` оставляет только файлы.

Набери `find players -type f`.

Должны найтись текстовые профили игроков и `players/.drafts/anna.tmp`.

Если нужны только папки:

```bash
find players -type d
```

Вывод:

```text
players
players/.drafts
players/america
players/asia
players/europe
```

Здесь уже наоборот: `-type d` оставляет только папки.

Набери `find players -type d`.

Должны быть видны `players`, `.drafts`, `america`, `asia`, `europe`.

## Не уходить слишком глубоко: `-maxdepth`

Иногда искать по всей глубине не нужно. Хочется посмотреть только верхнюю часть дерева.

```bash
find . -maxdepth 2 -type d
```

Вывод:

```text
.
./.cache
./chat
./configs
./items
./logs
./matches
./players
./players/.drafts
./players/america
./players/asia
./players/europe
```

`-maxdepth 2` говорит: не опускайся глубже двух уровней.

Набери `find . -maxdepth 2 -type d`.

Должны быть видны верхние папки набора и папки внутри `players`, но глубже `find` уже не уходит.

## Искать по пути: `-path`

Бывает, имя файла заранее не важно. Важно, где он лежит.

Например, нужно найти всё, что относится к Европе.

```bash
find players -path '*/europe/*'
```

Вывод:

```text
players/europe/anna.txt
players/europe/ivan.txt
players/europe/max.txt
```

Здесь `-path` проверяет не только имя файла, а весь путь целиком.

Набери `find players -path '*/europe/*'`.

Должны найтись:
- `players/europe/anna.txt`
- `players/europe/ivan.txt`
- `players/europe/max.txt`

## Искать по нескольким условиям: `-o`

Иногда одного шаблона мало. Например, нужно найти файлы двух разных типов сразу.

Для этого у `find` есть `-o`, то есть “или”.

```bash
find . -type f \( -name '*.csv' -o -path '*/europe/*' \)
```

Вывод:

```text
./items/anna_inventory.csv
./items/carlos_inventory.csv
./items/ivan_inventory.csv
./items/john_inventory.csv
./items/kenji_inventory.csv
./items/li_inventory.csv
./items/max_inventory.csv
./players/europe/anna.txt
./players/europe/ivan.txt
./players/europe/max.txt
```

Скобки здесь важны. Они говорят `find`, какие условия нужно рассматривать вместе.

Набери `find . -type f \( -name '*.csv' -o -path '*/europe/*' \)`.

Должны найтись:
- `./items/anna_inventory.csv`
- `./items/carlos_inventory.csv`
- `./items/ivan_inventory.csv`
- `./items/john_inventory.csv`
- `./items/kenji_inventory.csv`
- `./items/li_inventory.csv`
- `./items/max_inventory.csv`
- `./players/europe/anna.txt`
- `./players/europe/ivan.txt`
- `./players/europe/max.txt`

## Искать в заданной папке

Ещё один частый случай: ты уже знаешь, где искать, и не хочешь запускать поиск от корня набора.

```bash
find players/europe -name '*.txt'
```

Вывод:

```text
players/europe/anna.txt
players/europe/ivan.txt
players/europe/max.txt
```

Это тот же поиск по имени, но уже в конкретной папке.

Набери `find players/europe -name '*.txt'`.

Должны найтись:
- `players/europe/anna.txt`
- `players/europe/ivan.txt`
- `players/europe/max.txt`

## Искать по размеру: `-size`

Иногда задача звучит не “как называется файл?”, а “какие файлы совсем маленькие?” или “какие файлы тут самые тяжёлые?”.

Для этого у `find` есть `-size`.

```bash
find . -type f -size +1000c
```

Вывод:

```text
./logs/2026-02-20.log
./logs/2026-02-21.log
./logs/2026-02-22.log
./ops_snapshot_2026-02-22.zip
```

Здесь `+1000c` означает: больше 1000 байт.

Если наоборот нужны маленькие файлы:

```bash
find . -type f -size -200c
```

Должны найтись маленькие файлы вроде `server.ini`, профилей игроков, матчей и файлов из `items`.

Набери `find . -type f -size -200c`.

Должны найтись небольшие файлы из `items`, `matches`, `players`, а ещё `./configs/server.ini`.

Набери `find . -type f -size +1000c`.

Должны найтись:
- `./logs/2026-02-20.log`
- `./logs/2026-02-21.log`
- `./logs/2026-02-22.log`
- `./ops_snapshot_2026-02-22.zip`

## Искать пустые папки: `-empty`

Ещё один полезный случай: проверить, нет ли в наборе пустых папок.

```bash
find . -type d -empty
```

В текущем наборе вывод пустой.

Это тоже нормальный результат: `find` просто сообщает, что пустых папок здесь нет.

Набери `find . -type d -empty`.

Пустых папок найтись не должно.

## Что делать с найденным: `-exec`

До этого момента `find` только выводил найденные пути. Но часто нужно не просто найти, а сразу применить к результату другую команду.

Для этого у `find` есть `-exec`.

И это не случайная надстройка. По духу Unix `find` не только ищет, но и умеет сразу что-то сделать с найденным результатом.

Поэтому у `find` есть не одно действие, а несколько:

| действие | что делает |
| --- | --- |
| `-print` | вывести найденный путь |
| `-delete` | удалить найденный объект |
| `-exec` | выполнить команду |
| `-ls` | показать результат в стиле `ls` |

```bash
find players/europe -name '*.txt' -exec wc -l {} \;
```

Вывод:

```text
8 players/europe/anna.txt
8 players/europe/ivan.txt
8 players/europe/max.txt
```

Здесь:
- `wc -l` — команда, которую нужно выполнить;
- `{}` — специальная заглушка. `find` заменяет её на текущий найденный путь;
- `\;` — конец команды для `-exec`.

То есть `find` по очереди берёт каждый найденный файл и запускает для него `wc -l`.

Про `wc` подробно будет отдельный урок. Пока здесь достаточно понимать идею: `find` нашёл файл и сразу передал его в другую команду.

Здесь есть одна странная на вид часть: `\;`.

Многие в первый раз удивляются записи вроде:

```bash
find . -name '*.log' -exec rm {} \;
```

Почему именно `\;`?

История простая: в shell символ `;` уже означает разделитель команд. Поэтому его пришлось экранировать, чтобы он дошёл до `find` как обычный символ, а не как конец команды оболочки.

Это решение тянется ещё из старых Unix-времён, и оно до сих пор с нами.

Набери `find players/europe -name '*.txt' -exec wc -l {} \;`.

Должны появиться три строки, по одной на каждый файл из `players/europe`.

## Шпаргалка

- `find . -name '*.csv'` искать по имени.
- `find items -name '*.csv'` искать по расширению в одной папке.
- `find . -iname '*anna*'` искать по имени без учёта регистра.
- `find . -type f` оставить только файлы.
- `find . -type d` оставить только папки.
- `find . -maxdepth 2` ограничить глубину.
- `find . -path '*/europe/*'` искать по части пути.
- `find . \( A -o B \)` искать по условию “A или B”.
- `find players/europe -name '*.txt'` искать только внутри одной папки.
- `find . -type f -size +1000c` искать файлы больше 1000 байт.
- `find . -type d -empty` искать пустые папки.
- `find ... -exec command {} \;` запустить команду для каждого найденного пути.

## Напоследок

1. `find` очень старая команда.
Она появилась ещё в раннем Unix и пережила почти всю историю системы.

2. У `find` самый длинный синтаксис среди Unix-утилит.
Есть даже шутка, что `find` — это язык программирования, случайно оказавшийся в терминале.

## Практика

2.1. Найди все файлы, которые либо лежат в `players/europe`, либо заканчиваются на `.csv`.

   Должны найтись:
   - `./items/anna_inventory.csv`
   - `./items/carlos_inventory.csv`
   - `./items/ivan_inventory.csv`
   - `./items/john_inventory.csv`
   - `./items/kenji_inventory.csv`
   - `./items/li_inventory.csv`
   - `./items/max_inventory.csv`
   - `./players/europe/anna.txt`
   - `./players/europe/ivan.txt`
   - `./players/europe/max.txt`

2.2. Найди все файлы меньше 200 байт, но только среди `.txt` и `.csv`.

   Должны найтись:
   - `./.cache/index.txt`
   - `./items/anna_inventory.csv`
   - `./items/carlos_inventory.csv`
   - `./items/ivan_inventory.csv`
   - `./items/john_inventory.csv`
   - `./items/kenji_inventory.csv`
   - `./items/li_inventory.csv`
   - `./items/max_inventory.csv`
   - `./matches/match_1001.txt`
   - `./matches/match_1002.txt`
   - `./matches/match_1003.txt`
   - `./matches/match_1004.txt`
   - `./players/america/carlos.txt`
   - `./players/america/john.txt`
   - `./players/asia/kenji.txt`
   - `./players/asia/li.txt`
   - `./players/europe/anna.txt`
   - `./players/europe/ivan.txt`
   - `./players/europe/max.txt`

2.3. Найди все `.txt` файлы в `players/europe` и покажи их через действие `-ls`.

   Должны появиться три строки про:
   - `players/europe/anna.txt`
   - `players/europe/ivan.txt`
   - `players/europe/max.txt`

2.4. Найди все файлы меньше 200 байт, которые либо относятся к `anna`, либо лежат в `players/europe`, и запиши результат в `/tmp/find_special.txt`.

   В файле `/tmp/find_special.txt` должны оказаться:
   - `./items/anna_inventory.csv`
   - `./players/.drafts/anna.tmp`
   - `./players/europe/anna.txt`
   - `./players/europe/ivan.txt`
   - `./players/europe/max.txt`
