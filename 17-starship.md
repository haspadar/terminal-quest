# 17. Starship: настраиваем приглашение командной строки

Стандартный prompt показывает имя пользователя и папку. Starship показывает то же самое, но добавляет контекст: ветку git, версию языка, статус последней команды — и делает это быстро.

## Установка

На macOS:

```bash
brew install starship
```

На Linux:

```bash
curl -sS https://starship.rs/install.sh | sh
```

Затем добавьте строку в конфиг fish:

```bash
echo 'starship init fish | source' >> ~/.config/fish/config.fish
```

Перезапустите fish или выполните:

```bash
source ~/.config/fish/config.fish
```

Prompt изменится сразу.

## Что показывает по умолчанию

Перейдите в репозиторий:

```bash
cd terminal-quest
```

Prompt должен показать что-то вроде:

```
~/Projects/learning/terminal-quest on  main
❯
```

Starship автоматически определил git-репозиторий и показал ветку.

## Конфигурация

Конфиг живёт в `~/.config/starship.toml`. Создайте его:

```bash
mkdir -p ~/.config
touch ~/.config/starship.toml
```

### Изменить символ приглашения

```toml
[character]
success_symbol = '[→](green)'
error_symbol = '[→](red)'
```

После сохранения изменения вступят в силу в новой команде — перезапускать fish не нужно.

### Показать время выполнения команды

```toml
[cmd_duration]
min_time = 500
format = 'took [$duration](yellow) '
```

Теперь команды, которые выполнялись дольше 500 мс, будут показывать время.

Попробуйте:

```bash
sleep 1
```

Должно появиться `took 1s` рядом с prompt.

### Скрыть имя пользователя и хоста

По умолчанию Starship не показывает `user@host` — только если вы подключены по SSH. Если хотите убрать совсем:

```toml
[username]
disabled = true

[hostname]
disabled = true
```

## Готовые пресеты

Starship поставляется с несколькими готовыми наборами настроек:

```bash
starship preset nerd-font-symbols -o ~/.config/starship.toml
```

Посмотреть список доступных пресетов:

```bash
starship preset --list
```

## Шпаргалка

- `starship init fish | source` — подключить в fish.
- `~/.config/starship.toml` — файл конфигурации.
- `[character]` — символ приглашения.
- `[cmd_duration]` — время выполнения команды.
- `starship preset --list` — список готовых тем.

## Напоследок

- Starship написан на Rust и добавляет к отрисовке prompt меньше 5 мс — быстрее большинства аналогов на Python или shell.
- Конфиг в TOML-формате: можно держать в git и переносить между машинами одним файлом.

## Практика

17.1. Установите Starship и подключите его к fish. Убедитесь, что prompt изменился.

   При переходе в `terminal-quest` должна быть видна git-ветка.

17.2. Добавьте в `starship.toml` показ времени выполнения для команд дольше 200 мс. Проверьте на `sleep 1`.

   После `sleep 1` должно появиться время выполнения рядом с prompt.

17.3. Измените символ приглашения на любой другой. Проверьте, что он меняет цвет при ошибке.

   Запустите несуществующую команду (`foo`) и убедитесь, что символ стал красным.
