# Практическая работа: HTTP-запросы и Network

**Проверено:** 23.11.2026. Сайт: [httpbin.org](https://httpbin.org/) — сервис, который возвращает сведения о запросе и позволяет вызвать нужный статус.

## Три статус-кода

| Метод и адрес | Статус | Что означает |
|---|---:|---|
| `GET https://httpbin.org/get` | **200 OK** | Ресурс успешно получен. |
| `GET https://httpbin.org/status/201` | **201 Created** | Сервер вернул код успешного создания ресурса. Здесь код вызван тестовым адресом; сам запрос ничего не создаёт. |
| `GET https://httpbin.org/status/404` | **404 Not Found** | Сервер сообщает, что ресурс не найден. Здесь код вызван тестовым адресом. |

Статусы проверены реальными HTTP-запросами. Для просмотра именно в **DevTools → Network** откройте любой сайт, нажмите `F12`, выберите **Network**, включите **Preserve log**, затем по очереди откройте три адреса из таблицы в этой же вкладке. В списке запросов смотрите колонки **Method** и **Status**, а в карточке запроса — **Headers**.

## Методы HTTP

| Метод | Для чего используется | Свой пример |
|---|---|---|
| `GET` | Получить данные без изменения ресурса. | Получить расписание занятий на понедельник: `GET /api/schedule?day=monday`. |
| `POST` | Создать запись или запустить действие с переданными данными. | Добавить заявку на ремонт принтера: `POST /api/repair-requests` с описанием неисправности. |
| `PUT` | Заменить ресурс целиком по известному адресу. | Сохранить всю карточку кабинета: `PUT /api/rooms/214` с номером, этажом и вместимостью. |
| `PATCH` | Изменить часть существующего ресурса. | Поменять только вместимость кабинета: `PATCH /api/rooms/214` с полем `capacity`. |
| `DELETE` | Удалить ресурс. | Удалить отменённую бронь аудитории: `DELETE /api/bookings/731`. |

Адреса в примерах методов — придуманные для учебной системы; реальные запросы ниже отправлены на httpbin.org.

## Запрос с методом POST

**Адрес:** `https://httpbin.org/post`  
**Ответ:** `200 OK`  
**Отправленные заголовки:**

```http
Host: httpbin.org
User-Agent: curl/8.23.0
Accept: */*
Content-Type: application/json
Content-Length: 56
```

**Отправленное тело:**

```json
{"task":"network-practice","student":"Kirill","count":3}
```

Сервер вернул `method: "POST"` и те же значения в полях `headers` и `json`. Логин и пароль не использовались.

Для воспроизведения **POST в DevTools → Network** откройте `https://httpbin.org/`, затем в **Console** выполните:

```js
fetch('https://httpbin.org/post', {
  method: 'POST',
  headers: { 'Content-Type': 'application/json' },
  body: JSON.stringify({ task: 'network-practice', student: 'Kirill', count: 3 })
});
```

В **Network** откройте запрос `post`: **Headers → Request Headers** покажет заголовки, а **Payload → Request Payload** — тело. Браузер может добавить свои заголовки и иначе посчитать `Content-Length`, поэтому они могут отличаться от приведённого выше запроса `curl`.

## Команды проверки

```bash
curl -o /dev/null -w '%{http_code}\n' https://httpbin.org/get
curl -o /dev/null -w '%{http_code}\n' https://httpbin.org/status/201
curl -o /dev/null -w '%{http_code}\n' https://httpbin.org/status/404
curl -i -X POST https://httpbin.org/post -H 'Content-Type: application/json' --data '{"task":"network-practice","student":"Kirill","count":3}'
```
