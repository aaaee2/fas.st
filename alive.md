alive.fas.st
===================


Сервис `alive.fas.st` предназначен для проверки работоспособности `*.fas.st` из-вне. Т.е. это некая имитация публичного доступ с постороннего IP-адреса.

Ниже приведены возможные форматы запросов и ответы на них.

----------

JSON (по-умолчанию)
-------------------

URL: http://alive.fas.st
URL: http://alive.fas.st/json

Результат:

`Content-Type: application/json`

```javascript
[
  {
    "name": "короткое имя сервиса", /* string */
    "ok": true|false /* boolean, сервис прошел или не прошел проверку */
  },
  ...
]
```

XML
-------------------

URL: http://alive.fas.st/xml

Результат:

`Content-Type: application/xml`

```xml

<?xml version="1.0" encoding="UTF-8"?>
<results>
	<item>
		<name>короткое имя сервиса</name>
		<ok>true|false</ok>
	</item>
	...
</results>
```

TEXT
-------------------

URL: http://alive.fas.st/plain

Результат:

`Content-Type: text/plain`

```
короткое имя сервиса: true|false
...
короткое имя сервиса: true|false
```

По-другому можно описать как: `<короткое имя сервиса>: <true|false>[\n<короткое имя сервиса>: <true|false>[\n<короткое имя сервиса>: <true|false>]]`

> Формат перевода строк - UNIX (LF).


Что такое сервисы и что мы проверяем?
-------------------

Сервисы - это набор проверочных правил для поддоменов `fas.st` и их ключевых контроллеров (URI).
Ниже приведен реальный набор на момент написания данного документа:

> Блок `expected` означает какой код HTTP-статуса и контент мы ожидаем, чтобы считать, что данный сервис работоспособен.


```javascript
var services = [{
	name: 'www 200',
	url: 'https://fas.st',
	expected: {
		code: 200
	}
},{
	name: 'www 302',
	url: 'https://fas.st/-bIly',
	expected: {
		code: 302
	}
},{
	name: 'api ping',
	url: 'https://api.fas.st/ping',
	expected: {
		code: 200,
		raw: 'pong'
	}
},{
	name: 'api auth',
	url: 'https://api.fas.st/v1/pack',
	headers: null,
	expected: {
		code: 403
	}
},{
	name: 'api unpack',
	url: 'https://api.fas.st/v1/unpack',
	method: 'POST',
	body: {
		access_token: 'anonymous',
		username: 'website_visitor',
		'url': 'http://fas.st/1Lm49u'
	},
	headers: {
		'Authorization': 'www',
		'Content-Type': 'application/json'
	},
	expected: {
		code: 200,
		raw: '{"url":"https://developer.mozilla.org/en-US/Add-ons"}'
	}
},{
	name: 'adm 200',
	url: 'https://adm.fas.st/login',
	expected: {
		code: 200
	}
},{
	name: 'adm auth',
	url: 'https://adm.fas.st/',
	expected: {
		code: 302
	}
}];
```
