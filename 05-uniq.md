# 05. `uniq`: убираем соседние повторы

После `sort` появляется задача убрать повторы, посчитать их или оставить только строки, которые встречаются несколько раз. Для этого и нужен `uniq`.

`uniq` сравнивает строку только с предыдущей. Без `sort` он часто не находит ничего полезного.

Перейдите в папку:

```bash
cd game_server_intro
```

## Почему `uniq` почти всегда идёт после `sort`

Если одинаковые строки не стоят рядом, `uniq` их не заметит.

```bash
cat players/*/*.txt | uniq -d
```

Вывода не будет.

Теперь те же строки, но уже после сортировки:

```bash
cat players/*/*.txt | sort | uniq -d
```

Должны остаться только повторяющиеся строки:

- `clan=dragons`
- `clan=eagles`
- `class=warrior`
- `created_at=2026-02-19`
- `region=america`
- `region=asia`
- `region=europe`

Главное правило: сначала `sort`, потом `uniq`.

## Базовый запуск

Если подать один и тот же профиль дважды, `uniq` схлопнет дубли в одну строку.

```bash
cat players/europe/anna.txt players/europe/anna.txt | sort | uniq
```

Должны остаться строки из профиля `anna`, но без повторов:

- `clan=dragons`
- `class=healer`
- `created_at=2026-02-19`
- `email=anna@test.org`
- `gold=8200`
- `level=38`
- `nickname=anna`
- `region=europe`

`uniq` убирает подряд идущие одинаковые строки.

## Посчитать повторы: `-c`

Когда нужно не убрать повторы, а понять, сколько раз встретилась каждая строка, пригодится `-c`.

```bash
cat players/*/*.txt | sort | uniq -c
```

Перед строками должны появиться числа. Среди них должны быть:

- `7 created_at=2026-02-19`
- `3 region=europe`
- `2 region=asia`
- `2 region=america`
- `2 class=warrior`
- `2 clan=eagles`
- `2 clan=dragons`

Теперь `uniq` показывает количество повторов.

## Оставить только повторы: `-d`

Флаг `-d` оставляет только те строки, которые встретились больше одного раза.

```bash
cat players/*/*.txt matches/*.txt | sort | uniq -d
```

Должны остаться строки, которые повторяются в нескольких файлах:

- `clan=dragons`
- `clan=eagles`
- `class=warrior`
- `created_at=2026-02-19`
- `datacenter=eu-west`
- `mode=ranked`
- `region=america`
- `region=asia`
- `region=europe`

Это похоже на разведку: какие значения встречаются несколько раз.

## Шпаргалка

- `sort | uniq` убрать соседние повторы.
- `sort | uniq -c` посчитать повторы.
- `sort | uniq -d` оставить только строки с повторами.

## Напоследок

1. `uniq` — простая утилита: она сравнивает текущую строку с предыдущей и больше ничего.

2. В исходниках GNU Core Utilities основная логика `uniq` занимает несколько десятков строк.

## Практика

5.1. Выведите файл `players/europe/anna.txt` два раза подряд, потом отсортируйте строки и уберите повторы.

   Должны остаться:

   - `clan=dragons`
   - `class=healer`
   - `created_at=2026-02-19`
   - `email=anna@test.org`
   - `gold=8200`
   - `level=38`
   - `nickname=anna`
   - `region=europe`

5.2. Через `find`, `sort` и `uniq` покажите только строки из профилей игроков, которые встречаются больше одного раза.

   Должны остаться:

   - `clan=dragons`
   - `clan=eagles`
   - `class=warrior`
   - `created_at=2026-02-19`
   - `region=america`
   - `region=asia`
   - `region=europe`

5.3. Через `find`, `uniq -c` и `wc` посчитайте число разных строк в профилях игроков.

   Должно появиться число `43`.

5.4. Через `find`, `uniq -c` и `sort` посчитайте повторы в профилях игроков так, чтобы самые частые строки были сверху.

   Наверху должны оказаться:

   - `7 created_at=2026-02-19`
   - `3 region=europe`
   - `2 region=asia`
   - `2 region=america`
   - `2 class=warrior`
   - `2 clan=eagles`
   - `2 clan=dragons`
