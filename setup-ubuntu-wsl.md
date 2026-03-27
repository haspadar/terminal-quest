# Ubuntu / WSL

Эта настройка подходит для Ubuntu и WSL с Ubuntu.

## Базовые пакеты

```bash
sudo apt update
sudo apt install fish git curl jq tree unzip nodejs npm fd-find fzf
```

Должны установиться shell, git, сетевые утилиты и инструменты для первых уроков.

## `fd` в Ubuntu

На Ubuntu пакет называется `fd-find`, а команда часто доступна как `fdfind`.

Проверьте:

```bash
fdfind --version
```

Если хотите использовать короткое имя `fd`, добавьте алиас в `fish`:

```fish
echo 'alias fd=fdfind' >> ~/.config/fish/config.fish
source ~/.config/fish/config.fish
```

## `fzf` в Ubuntu

Пакет из `apt` может оказаться старым. Если в уроке про `fzf` команда `fzf --fish` не работает, используйте отдельную установку из самого урока.

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
