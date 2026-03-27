# Arch Linux

Эта настройка подходит для Arch Linux и совместимых систем.

## Базовые пакеты

```bash
sudo pacman -Syu --needed fish git curl jq tree unzip nodejs npm fd fzf
```

Должны установиться shell, git, сетевые утилиты и инструменты для первых уроков.

## `fisher` для урока про `z`

В уроке про `z` используется плагин `jethrokuan/z` для `fish`. Для него нужен менеджер плагинов `fisher`.

Если `fisher` ещё не установлен, выполните:

```fish
curl -sL https://raw.githubusercontent.com/jorgebucaran/fisher/main/functions/fisher.fish | source
fisher install jorgebucaran/fisher
```

## Репозиторий курса

```bash
git clone https://github.com/haspadar/terminal-quest.git
cd terminal-quest
```

## Примечание по дополнительным утилитам

`eza` и `starship` ставятся отдельно в своих уроках. Для Arch они обычно доступны прямо через `pacman`.
