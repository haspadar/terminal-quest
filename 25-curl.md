# 25. `curl`: делаем HTTP-запросы из терминала

В прошлых уроках вы смотрели запросы в браузере. `curl` делает то же самое — но из терминала, без браузера. Это удобно для скриптов, автоматизации и работы с API.

## Установка

В WSL Ubuntu curl обычно уже есть. Проверьте:

```fish
curl --version
```

Если нет:

```fish
sudo apt install curl
```

## Базовый GET-запрос

```fish
curl https://httpbin.org/get
```

Сервер вернёт JSON с данными вашего запроса. Это то же самое, что открыть URL в браузере — только в терминале.

Вывод будет слеплен в одну строку. Чтобы читать удобно — pipe в `jq`:

```fish
curl -s https://httpbin.org/get | jq '.'
```

Флаг `-s` убирает прогресс-бар, который curl показывает по умолчанию. В скриптах и с pipe всегда используйте `-s`.

## Посмотреть заголовки ответа

`-i` показывает заголовки ответа перед телом:

```fish
curl -i https://httpbin.org/get
```

Вверху появятся строки вида `HTTP/2 200`, `Content-Type: application/json` и другие. Ниже — тело ответа.

Только заголовки без тела — `-I`:

```fish
curl -I https://httpbin.org/get
```

## Проверить статус-код

Чтобы увидеть только код ответа:

```fish
curl -s -o /dev/null -w "%{http_code}\n" https://httpbin.org/status/200
curl -s -o /dev/null -w "%{http_code}\n" https://httpbin.org/status/404
curl -s -o /dev/null -w "%{http_code}\n" https://httpbin.org/status/500
```

`-o /dev/null` выбрасывает тело ответа, `-w "%{http_code}"` выводит только код. Должны появиться `200`, `404`, `500`.

## Следовать редиректам: -L

Без `-L` curl останавливается на редиректе. Попробуйте:

```fish
curl -s -o /dev/null -w "%{http_code}\n" https://httpbin.org/redirect/1
```

Должен появиться `302`. Теперь с `-L`:

```fish
curl -s -L -o /dev/null -w "%{http_code}\n" https://httpbin.org/redirect/1
```

Должен появиться `200` — curl прошёл по редиректу до конца.

## Указать заголовок: -H

`-H` добавляет заголовок к запросу. Посмотрим, что получит сервер:

```fish
curl -s -H "User-Agent: AlbionBot/1.0" https://httpbin.org/headers | jq '.headers["User-Agent"]'
```

Должно появиться `"AlbionBot/1.0"` — сервер видит именно то, что мы передали.

## Сохранить ответ в файл: -o

```fish
curl -s https://httpbin.org/get -o response.json
```

Файл появится в текущей папке. Проверьте:

```fish
jq '.' response.json
```

## POST-запрос: -X и -d

`-X POST` меняет метод, `-d` передаёт тело запроса:

```fish
curl -s -X POST \
  -H "Content-Type: application/json" \
  -d '{"city": "Caerleon", "price": 46991}' \
  https://httpbin.org/post | jq '.json'
```

В поле `"json"` ответа должны появиться данные, которые вы отправили. Сервер вернул их обратно — так httpbin подтверждает, что получил запрос правильно.

## Albion Online API

Теперь — настоящий запрос. Получите актуальные цены T6_BAG из API:

```fish
curl -s "https://www.albion-online-data.com/api/v2/stats/prices/T6_BAG?locations=Caerleon,Bridgewatch&qualities=1" | jq '.'
```

Должны появиться записи с реальными ценами — живые данные прямо из игры.

Найдите самый дешёвый город прямо в одной команде:

```fish
curl -s "https://www.albion-online-data.com/api/v2/stats/prices/T6_BAG?locations=Caerleon,Bridgewatch,Martlock,Thetford,Fort+Sterling,Lymhurst&qualities=1" \
  | jq 'min_by(.sell_price_min) | {city, price: .sell_price_min}'
```

А сохранить свежие данные в файл — одна строка:

```fish
curl -s "https://www.albion-online-data.com/api/v2/stats/prices/T6_BAG?locations=Caerleon,Bridgewatch,Martlock,Thetford,Fort+Sterling,Lymhurst&qualities=1" \
  -o albion/prices/today_T6_BAG.json
```

## Шпаргалка

- `curl -s URL` — GET-запрос без прогресс-бара.
- `curl -s URL | jq '.'` — GET с красивым выводом JSON.
- `curl -i URL` — заголовки + тело ответа.
- `curl -I URL` — только заголовки.
- `curl -s -o /dev/null -w "%{http_code}\n" URL` — только статус-код.
- `curl -L URL` — следовать редиректам.
- `curl -H "Key: Value" URL` — добавить заголовок.
- `curl -o file.json URL` — сохранить ответ в файл.
- `curl -X POST -H "Content-Type: application/json" -d '{}' URL` — POST с JSON.

## Напоследок

- `curl` создал Даниэль Стенберг в 1997 году. Изначально утилита называлась httpget, потом urlget — и наконец curl (Client URL). Сегодня curl встроен в Windows, macOS и большинство Linux-дистрибутивов.
- `curl` поддерживает не только HTTP — он умеет работать с FTP, SSH, SMTP и ещё двумя десятками протоколов. Но для работы с API нужен только HTTP.

## Практика

25.1. Запросите `https://httpbin.org/get` и найдите в ответе поле `"origin"` — ваш IP-адрес.

   Используйте `curl -s URL | jq '.origin'`.

25.2. Проверьте статус-коды для трёх URL: `httpbin.org/status/200`, `/status/404`, `/status/503`.

   Используйте `-o /dev/null -w "%{http_code}\n"`. Должны появиться `200`, `404`, `503`.

25.3. Запросите актуальные цены T6_BAG по всем городам из Albion API и найдите город с максимальной ценой продажи.

   Используйте `curl -s URL | jq 'max_by(.sell_price_min) | {city, price: .sell_price_min}'`.
