# 08. `printf`: выводим по шаблону

`echo` хорош для быстрой проверки. Но как только нужен предсказуемый вывод с одним и тем же шаблоном, удобнее взять `printf`.

`printf` полезен, когда надо:

- вывести значение в точной форме;
- повторить один и тот же шаблон для нескольких значений;
- выровнять вывод по столбцам.

Перейдите в папку:

```bash
cd game_server_intro
```

Дальше все команды запускайте внутри неё.

## Базовая форма

У `printf` есть форматная строка и значения:

```bash
printf FORMAT VALUES
```

Самый частый вариант:

```bash
printf '%s\n' players/europe
```

Должен появиться путь `players/europe`.

`%s` значит “подставь строку”, а `\n` добавляет перевод строки.

Наберите:

```bash
printf 'scan: %s\n' players/europe
```

Должна появиться строка `scan: players/europe`.

## Вывести переменную по шаблону

`printf` особенно полезен после переменной: шаблон остаётся неизменным, а значение можно менять.

```bash
set folder players/europe
printf 'scan: %s\n' $folder
```

Должна появиться строка `scan: players/europe`.

Наберите:

```bash
set stats (wc -l chat/global_chat.log)
printf 'chat stats: %s\n' "$stats"
```

Должна появиться строка, где есть `chat stats:` и `7 chat/global_chat.log`.

## Повторить один шаблон для списка

Это главное отличие от `echo`. Один формат можно применить сразу ко всем значениям.

```bash
set files (find players/europe -name '*.txt' | sort)
printf 'profile: %s\n' $files
```

Должны быть видны:

- `profile: players/europe/anna.txt`
- `profile: players/europe/ivan.txt`
- `profile: players/europe/max.txt`

`printf` повторяет форматную строку для каждого элемента списка.

Наберите:

```bash
set logs (find logs -name '*.log' | sort)
printf 'log: %s\n' $logs
```

Должны быть видны:

- `log: logs/2026-02-20.log`
- `log: logs/2026-02-21.log`
- `log: logs/2026-02-22.log`

## Перевод строки не добавляется сам

В отличие от `echo`, `printf` не делает `\n` автоматически.

```bash
printf 'hello'
echo world
```

Должна появиться строка `helloworld`.

Поэтому в шаблоне перевод строки обычно пишут явно.

Наберите:

```bash
printf 'hello\n'
printf 'world\n'
```

Должны быть видны:

- `hello`
- `world`

## Выравнивание по ширине

`printf` умеет выравнивать значения по ширине.

```bash
printf '%-8s %s\n' anna 38 ivan 42 max 27
```

Должны быть видны:

- `anna     38`
- `ivan     42`
- `max      27`

Здесь `%-8s` значит: вывести строку и занять под неё 8 символов.

Наберите:

```bash
printf '%-12s %s\n' europe players/europe logs logs
```

Должны быть видны:

- `europe       players/europe`
- `logs         logs`

## Когда `printf` лучше `echo`

Если нужен просто быстрый текст на экран, хватит `echo`.

Если нужен точный формат, одинаковый префикс на каждой строке, явный `\n` или выравнивание, лучше сразу брать `printf`. Поэтому в переносимых скриптах используют `printf`.

## Шпаргалка

- `printf '%s\n' $name` вывести значение с переводом строки.
- `printf 'x: %s\n' $name` вывести подпись и значение.
- `printf 'x: %s\n' $list` применить один шаблон ко всем элементам списка.
- `printf '%-8s %s\n' a b` выровнять вывод по ширине.

## Напоследок

1. `printf` пришёл в shell из языка C. Идея с форматной строкой и подстановками вроде `%s` выросла именно оттуда.

2. `printf` не добавляет перевод строки автоматически. Если нужен перенос, его всегда пишут явно, например `printf '%s\n' hello`. Команда выглядит чуть длиннее, зато вывод получается полностью предсказуемым.

## Практика

8.1. Сохраните в переменную отсортированный список `.log` файлов и выведите каждый путь через `printf` с префиксом `log:`.

   Должны быть видны:

   - `log: logs/2026-02-20.log`
   - `log: logs/2026-02-21.log`
   - `log: logs/2026-02-22.log`

8.2. Сохраните в переменную отсортированный список всех `.txt` профилей игроков и выведите каждый путь через `printf` с префиксом `profile:`.

   Должны быть видны:

   - `profile: players/america/carlos.txt`
   - `profile: players/america/john.txt`
   - `profile: players/asia/kenji.txt`
   - `profile: players/asia/li.txt`
   - `profile: players/europe/anna.txt`
   - `profile: players/europe/ivan.txt`
   - `profile: players/europe/max.txt`

8.3. Сохраните в одну переменную отсортированный список файлов из `players/europe` и `items`, в другую переменную сохраните результат `wc -c` для этого списка и выведите каждую строку через `printf` с префиксом `bytes:`.

   Должны быть видны:

   - `bytes:      106 items/anna_inventory.csv`
   - `bytes:      107 items/carlos_inventory.csv`
   - `bytes:      131 items/ivan_inventory.csv`
   - `bytes:      102 items/john_inventory.csv`
   - `bytes:      109 items/kenji_inventory.csv`
   - `bytes:       95 items/li_inventory.csv`
   - `bytes:       96 items/max_inventory.csv`
   - `bytes:      115 players/europe/anna.txt`
   - `bytes:      125 players/europe/ivan.txt`
   - `bytes:      113 players/europe/max.txt`
   - `bytes:     1099 total`
