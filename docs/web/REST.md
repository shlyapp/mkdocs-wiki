# REST api

## Что это?

> REST API - архитектурный стиль для распределенных гипермедиа систем.

## Основные принципы REST API

### 1.Client-Server - Отделение клиента от сервера

Интерфейс является отдельной программой от сервера с бизнес логикой и базой данных.

### 2.Stateless - Отсутствие записи состояния клиента

Сервер не хранит и не запоминает состояние клиента. Каждый запрос понятен в отрыве от контекста.

### 3.Cacheable - Кэшируемость

В сообщении может быть пометка, что данные можно закешировать и обращаться к ним из памяти.

### 4.Uniform interface - Унификация интерфейса

#### Identification of resources - Идентификация ресурса

Каждый ресурс должен иметь уникальный идентификатор.

#### Manipulation through representation - Манипуляции через представление

Клиент предоставляет серверу ресурс в уже измененном виду. А сервер уже решает применить эти изменения или нет.

#### Self-descriptive messages - Самоописанные сообщения

Данных в сообщении достаточно, чтобы их понять и интерпретировать.

#### Hypermedia as the engine of application state - Гипермедиа как двигатель состояния

Сервер подсказывает клиенту какие дальнейшие запросы он может делать.

### 5.Layered - Многоуровневость

Сервер может быть большое множество. Но каждый знает только о существовании ближайшего.

### 6.Code of demand - Код по запросу

Сервер может отправить на клиент код для непосредственного исполнения.

## Шпаргалка

### Принципы построения URL

Для описания ресурса используй существительное во множественном числе. <br />

`/api/posts` - коллекция ресурсов <br />
`/api/posts/42` - конкретный ресурс <br />

URL может быть составным, если ресурс существует в контексте другого ресурса. <br />

`/api/posts/42/comments` - коллекция ресурсов <br /> 
`/api/posts/32/comments/12` - конкретный ресурс  <br />


### Методы запроса

`GET` - получить ресурс или коллекция  <br />
`POST` - создание или запрос без сайд-эффекта <br />
`PUT` - обновление  <br />
`DELETE` - удаление  <br />


### Заголовки запроса

`Accept` - типы медиа, разрешенные в ответе  <br />
`Authorization` - авторизационные данные для запроса <br /> 
`Cache-Control` - определяет, разрешено ли кэширование  <br />
`Content-Type` - тип тела запроса  <br />
`User-Agent` - ПО, при помощи которого осуществляется запрос - например, бразуер  <br />


### HTTP статус коды

`1xx` - информационные  <br />
`2xx` - успешные  <br />
`3xx` - перенаправление <br />
`4xx` - ошибки запроса  <br />
`5xx` - ошибки сервера  <br />

### Заголовки ответа

`Cache-Control` - определяет, можно ли кэшировать запрос, и как долго <br /> 
`Content-Encoding` - тип кодировки данных  <br />
`Content-Type` - формат ответа  <br />
`Location` - определяет, был ли запрос перенаправлен, или где создавался ресурс  <br />

## RESTful API

Это API, которые использует принципы проектирования REST

