# FASST. Интерфейс программирования приложений (API)

FASST представляет собой сервис, предназначенный для сжатия длинных web-ссылок. Поддерживаемые протоколы: 

  - http
  - https
  - feed

На момент написания данного документа существуют три контроллера:
  - //api.fas.st/v1/pack
  - //api.fas.st/v1/unppack
  - //api.fas.st/ping

## Версия
0.0.1 от 2016-04-29.

## Авторизация

Существуют два метода авторизации в FASST.

 - `accept` - не требует идентификационных данных или HTTP-заголовков, безусловное разрешение на использование;
 - `identify` - требует HTTP-заголовок `Authorization`, содержащий ключевое слово (ключ) которое соответствует имени сервиса клиента FASST API.

Пример авториазции по типу `identify`:
```sh
curl -X POST -H "Authorization: www" -H "Content-Type: application/json" -d '{"access_token":"anonymous","username":"website_visitor","url":"https://www.ripe.net/about-us/what-we-do"}' "http://api.fas.st/v1/pack"
```

## Методы API

# /v1/pack

Метод предназначен для сжатия длинного URL типа `(http|https|feed)://<domain>.<zone>/[path...]` в короткий URL FASST.

Авторизация - `identify`. Метод передачи данных - `POST`.

HTTP-заголовки:

| Наименование  | Содержимое                               |
|---------------|------------------------------------------|
| Content-Type  | application/json                         |
| Authorization | <имя или ключевое слово сервиса клиента> |


Данные в формате JSON:

| Параметр      | Значение                                 |
|---------------|------------------------------------------|
| param1        | <идентификатор1> в backend клиента       |
| param2        | <идентификатор2> в backend клиента       |
| ...           | ...                                      |
| paramN        | <идентификаторN> в backend клиента       |
| url           | <url для сжатия>                         |

Пример данных:
```json
{
    "access_token": "foo",
    "username": "bar",
    "url": "http://example.com/very/long/url"
}
```

# /v1/unpack

Метод предназначен для преобразования короткого URL FASST в оригинальный URL.

Авторизация - `identify`. Метод передачи данных - `POST`.

HTTP-заголовки:

| Наименование  | Содержимое                               |
|---------------|------------------------------------------|
| Content-Type  | application/json                         |
| Authorization | <имя или ключевое слово сервиса клиента> |


Данные в формате JSON:

| Параметр      | Значение                                 |
|---------------|------------------------------------------|
| param1        | <идентификатор1> в backend клиента       |
| param2        | <идентификатор2> в backend клиента       |
| ...           | ...                                      |
| paramN        | <идентификаторN> в backend клиента       |
| url           | <url для сжатия>                         |

Пример данных:
```json
{
    "access_token": "foo",
    "username": "bar",
    "url": "http://fas.st/1Lm49u"
}
```

# /ping

Метод предназначен для проверки отклика FASST API.

Авторизация - `accept`. Данные не требуются. Заголовки не требуются.

Единственный возможный ответ сервера:
```sh
HTTP/1.1 200 OK
...
Content-Type: text/html; charset=utf-8
Content-Length: 4
Connection: keep-alive
..

pong
```