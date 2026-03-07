# 06. `echo`, `printf`, `set` и `for`: сохраняем, выводим, повторяем

Перед тобой всё та же папка `game_server_intro`. После `find`, `wc` и `sort` быстро появляется новая боль: одни и те же команды и куски вывода приходится печатать снова и снова. А иногда ещё нужно одну и ту же команду запустить для нескольких значений подряд.

Здесь пригодятся четыре вещи:
- `set` сохранить значение;
- `echo` быстро показать его на экран;
- `printf` вывести аккуратно и предсказуемо;
- `for` повторить одну и ту же команду для нескольких значений.

Перейди в папку:

```bash
cd game_server_intro
```

## `set`: записать значение в переменную

В `fish` переменная создаётся через `set`.

```bash
set folder players/europe
echo $folder
```

Должно быть: `players/europe`.

Теперь путь лежит в переменной `folder`, и его можно подставлять в другие команды.

Например:

```bash
set folder players/europe
find $folder -name '*.txt'
```

Должно быть:
`players/europe/anna.txt`
`players/europe/ivan.txt`
`players/europe/max.txt`

## `set`: сохранить вывод команды

В `fish` можно положить в переменную не только готовый текст, но и вывод другой команды.

```bash
set europe_players (find players/europe -name '*.txt' | sort)
echo $europe_players
```

Должно быть:
`players/europe/anna.txt players/europe/ivan.txt players/europe/max.txt`

Скобки `(...)` здесь означают: “сначала выполни команду, потом сохрани её вывод в переменную”.

## `echo`: быстро показать значение

`echo` хорош, когда нужно просто быстро проверить переменную или показать то, что уже вернула другая команда.

```bash
set sorted_profiles (find players -name '*.txt' | sort)
echo $sorted_profiles
```

Должно быть: на экране идёт отсортированный список файлов из `players`.

`echo` удобен для быстрых проверок, но с форматированием у него всё скромно.

## `printf`: вывести по шаблону

Когда нужно не просто показать текст, а собрать его аккуратно и предсказуемо, лучше брать `printf`.

```bash
set lines (wc -l chat/global_chat.log)
printf 'chat stats: %s\n' $lines
```

Должно быть: строка начинается с `chat stats:` и дальше идёт вывод `wc -l` для `chat/global_chat.log`.

Здесь `%s` означает “подставь строку”, а `\n` переводит курсор на новую строку.

Ещё пример:

```bash
set folder players/europe
printf 'scan folder: %s\n' $folder
```

Должно быть: `scan folder: players/europe`.

Коротко разница такая:
- `echo` быстрее и проще;
- `printf` лучше, когда важен точный формат вывода.

## `for`: повторить команду несколько раз

Когда в переменной лежит список значений, удобно пройти по нему циклом `for`.

```bash
set europe_players (find players/europe -name '*.txt' | sort)

for file in $europe_players
    echo $file
end
```

Должно быть:
`players/europe/anna.txt`
`players/europe/ivan.txt`
`players/europe/max.txt`

Такой цикл говорит: бери по одному значению из списка и выполняй команды внутри блока.

Можно сразу сочетать `for` и `printf`:

```bash
set europe_players (find players/europe -name '*.txt' | sort)

for file in $europe_players
    printf 'profile: %s\n' $file
end
```

Должно быть:
`profile: players/europe/anna.txt`
`profile: players/europe/ivan.txt`
`profile: players/europe/max.txt`

Ещё один полезный вариант: пройти циклом по уже отсортированному списку файлов.

```bash
set profiles (find players -name '*.txt' | sort)

for profile in $profiles
    printf 'profile: %s\n' $profile
end
```

Должно быть:
`profile: players/america/carlos.txt`
`profile: players/america/john.txt`
`profile: players/asia/kenji.txt`
...

## Шпаргалка

- `set name value` записать значение в переменную.
- `set name (command)` сохранить вывод команды.
- `echo $name` быстро вывести значение.
- `printf 'x=%s\n' $name` вывести значение по шаблону.
- `for x in $list ... end` пройти по всем значениям из списка.

## Практика

1. Сохрани в переменную путь `players/europe` и используй её в `find`, чтобы показать все `.txt` файлы в этой папке.

Должно быть:
`players/europe/anna.txt`
`players/europe/ivan.txt`
`players/europe/max.txt`

2. Сохрани в переменную вывод команды `find players/europe -name '*.txt' | sort` и выведи эту переменную через `echo`.

Должно быть:
`players/europe/anna.txt players/europe/ivan.txt players/europe/max.txt`

3. Сохрани в переменную вывод команды `wc -l chat/global_chat.log` и выведи его через `printf` с префиксом `chat stats:`.

Должно быть:
строка начинается с `chat stats:` и дальше идёт вывод `wc -l` для `chat/global_chat.log`.

4. Сохрани в переменную отсортированный список файлов из `players` и через `for` выведи его построчно с префиксом `profile:`.

Должно быть:
`profile: players/america/carlos.txt`
`profile: players/america/john.txt`
`profile: players/asia/kenji.txt`
...
