# Снапшот документации restful-booker

Снято **29.08.2026** со страницы `https://restful-booker.herokuapp.com/apidoc/index.html`
(исходные данные — `apidoc/api_data.js`). Стенд бесплатный и может исчезнуть в любой момент,
поэтому спецификация зафиксирована в репозитории: без неё тест-кейсы и баг-репорты
потеряют источник требований.

> Страница документации рендерится JavaScript'ом, поэтому обычная выгрузка HTML отдаёт
> только «Loading…». Машиночитаемый источник — `apidoc/api_data.js`.

Расхождения этой документации с фактическим поведением сервиса разобраны в
[bug-reports.md](bug-reports.md), сводно — в BUG-20.

## Оглавление

- [`POST /auth` — CreateToken](#createtoken)
- [`POST /booking` — CreateBooking](#createbooking)
- [`GET /booking` — GetBookingIds](#getbookings)
- [`DELETE /booking/1` — DeleteBooking](#deletebooking)
- [`GET /booking/:id` — GetBooking](#getbooking)
- [`PATCH /booking/:id` — PartialUpdateBooking](#partialupdatebooking)
- [`PUT /booking/:id` — UpdateBooking](#updatebooking)
- [`GET /ping` — HealthCheck](#ping)

---

<a id="createtoken"></a>
## `POST /auth` — CreateToken

Creates a new auth token to use for access to the PUT and DELETE /booking

**Заголовки — Header**

| Поле | Тип | Обязательное | Описание |
|---|---|---|---|
| `Content-Type` | string | **да** | Sets the format of payload you are sending |

**Параметры — Request body**

| Поле | Тип | Обязательное | Описание |
|---|---|---|---|
| `username` | String | **да** | Username for authentication |
| `password` | String | **да** | Password for authentication |

**Ответ — Success 200**

| Поле | Тип | Описание |
|---|---|---|
| `token` | String | Token to use in future requests |

**Пример запроса — Example 1:**

```bash
curl -X POST \
  https://restful-booker.herokuapp.com/auth \
  -H 'Content-Type: application/json' \
  -d '{
    "username" : "admin",
    "password" : "password123"
}'
```

**Пример ответа — Response:**

```http
HTTP/1.1 200 OK

{
    "token": "abc123"
}
```

---

<a id="createbooking"></a>
## `POST /booking` — CreateBooking

Creates a new booking in the API

**Заголовки — Header**

| Поле | Тип | Обязательное | Описание |
|---|---|---|---|
| `Content-Type` | string | **да** | Sets the format of payload you are sending. Can be application/json or text/xml |
| `Accept` | string | **да** | Sets what format the response body is returned in. Can be application/json or application/xml |

**Параметры — Request body**

| Поле | Тип | Обязательное | Описание |
|---|---|---|---|
| `firstname` | String | **да** | Firstname for the guest who made the booking |
| `lastname` | String | **да** | Lastname for the guest who made the booking |
| `totalprice` | Number | **да** | The total price for the booking |
| `depositpaid` | Boolean | **да** | Whether the deposit has been paid or not |
| `bookingdates.checkin` | Date | **да** | Date the guest is checking in |
| `bookingdates.checkout` | Date | **да** | Date the guest is checking out |
| `additionalneeds` | String | **да** | Any other needs the guest has |

**Ответ — Success 200**

| Поле | Тип | Описание |
|---|---|---|
| `bookingid` | Number | ID for newly created booking |
| `booking` | Object | Object that contains |
| `booking.firstname` | String | Firstname for the guest who made the booking |
| `booking.lastname` | String | Lastname for the guest who made the booking |
| `booking.totalprice` | Number | The total price for the booking |
| `booking.depositpaid` | Boolean | Whether the deposit has been paid or not |
| `booking.bookingdates` | Object | Sub-object that contains the checkin and checkout dates |
| `booking.bookingdates.checkin` | Date | Date the guest is checking in |
| `booking.bookingdates.checkout` | Date | Date the guest is checking out |
| `booking.additionalneeds` | String | Any other needs the guest has |

**Пример запроса — JSON example usage:**

```bash
curl -X POST \
  https://restful-booker.herokuapp.com/booking \
  -H 'Content-Type: application/json' \
  -d '{
    "firstname" : "Jim",
    "lastname" : "Brown",
    "totalprice" : 111,
    "depositpaid" : true,
    "bookingdates" : {
        "checkin" : "2018-01-01",
        "checkout" : "2019-01-01"
    },
    "additionalneeds" : "Breakfast"
}'
```

**Пример запроса — XML example usage:**

```bash
curl -X POST \
  https://restful-booker.herokuapp.com/booking \
  -H 'Content-Type: text/xml' \
  -d '<booking>
    <firstname>Jim</firstname>
    <lastname>Brown</lastname>
    <totalprice>111</totalprice>
    <depositpaid>true</depositpaid>
    <bookingdates>
      <checkin>2018-01-01</checkin>
      <checkout>2019-01-01</checkout>
    </bookingdates>
    <additionalneeds>Breakfast</additionalneeds>
  </booking>'
```

**Пример запроса — URLencoded example usage:**

```bash
curl -X POST \
  https://restful-booker.herokuapp.com/booking \
  -H 'Content-Type: application/x-www-form-urlencoded' \
  -d 'firstname=Jim&lastname=Brown&totalprice=111&depositpaid=true&bookingdates%5Bcheckin%5D=2018-01-01&bookingdates%5Bcheckout%5D=2018-01-02'
```

**Пример ответа — JSON Response:**

```http
HTTP/1.1 200 OK

{
    "bookingid": 1,
    "booking": {
        "firstname": "Jim",
        "lastname": "Brown",
        "totalprice": 111,
        "depositpaid": true,
        "bookingdates": {
            "checkin": "2018-01-01",
            "checkout": "2019-01-01"
        },
        "additionalneeds": "Breakfast"
    }
}
```

**Пример ответа — XML Response:**

```http
HTTP/1.1 200 OK

<?xml version='1.0'?>
<created-booking>
    <bookingid>1</bookingid>
    <booking>
        <firstname>Jim</firstname>
        <lastname>Brown</lastname>
        <totalprice>111</totalprice>
        <depositpaid>true</depositpaid>
        <bookingdates>
            <checkin>2018-01-01</checkin>
            <checkout>2019-01-01</checkout>
        </bookingdates>
        <additionalneeds>Breakfast</additionalneeds>
    </booking>
</created-booking>
```

**Пример ответа — URL Response:**

```http
HTTP/1.1 200 OK

bookingid=1&booking%5Bfirstname%5D=Jim&booking%5Blastname%5D=Brown&booking%5Btotalprice%5D=111&booking%5Bdepositpaid%5D=true&booking%5Bbookingdates%5D%5Bcheckin%5D=2018-01-01&booking%5Bbookingdates%5D%5Bcheckout%5D=2019-01-01
```

---

<a id="getbookings"></a>
## `GET /booking` — GetBookingIds

Returns the ids of all the bookings that exist within the API. Can take optional query strings to search and return a subset of booking ids.

**Параметры — Parameter**

| Поле | Тип | Обязательное | Описание |
|---|---|---|---|
| `firstname` | String | нет | Return bookings with a specific firstname |
| `lastname` | String | нет | Return bookings with a specific lastname |
| `checkin` | date | нет | Return bookings that have a checkin date greater than or equal to the set checkin date. Format must be CCYY-MM-DD |
| `checkout` | date | нет | Return bookings that have a checkout date greater than or equal to the set checkout date. Format must be CCYY-MM-DD |

**Ответ — Success 200**

| Поле | Тип | Описание |
|---|---|---|
| `object` | object[] | Array of objects that contain unique booking IDs |
| `object.bookingid` | number | ID of a specific booking that matches search criteria |

**Пример запроса — Example 1 (All IDs):**

```bash
curl -i https://restful-booker.herokuapp.com/booking
```

**Пример запроса — Example 2 (Filter by name):**

```bash
curl -i https://restful-booker.herokuapp.com/booking?firstname=sally&lastname=brown
```

**Пример запроса — Example 3 (Filter by checkin/checkout date):**

```bash
curl -i https://restful-booker.herokuapp.com/booking?checkin=2014-03-13&checkout=2014-05-21
```

**Пример ответа — Response:**

```http
HTTP/1.1 200 OK

[
  {
    "bookingid": 1
  },
  {
    "bookingid": 2
  },
  {
    "bookingid": 3
  },
  {
    "bookingid": 4
  }
]
```

---

<a id="deletebooking"></a>
## `DELETE /booking/1` — DeleteBooking

Deletes a booking from the API. Requires an authorization token to be set in the header or a Basic auth header.

**Заголовки — Header**

| Поле | Тип | Обязательное | Описание |
|---|---|---|---|
| `Cookie` | string | нет | Sets an authorization token to access the DELETE endpoint, can be used as an alternative to the Authorization |
| `Authorization` | string | нет | YWRtaW46cGFzc3dvcmQxMjM=]   Basic authorization header to access the DELETE endpoint, can be used as an alternative to the Cookie header |

**Параметры — Url Parameter**

| Поле | Тип | Обязательное | Описание |
|---|---|---|---|
| `id` | Number | **да** | ID for the booking you want to update |

**Ответ — Success 200**

| Поле | Тип | Описание |
|---|---|---|
| `OK` | String | Default HTTP 201 response |

**Пример запроса — Example 1 (Cookie):**

```bash
curl -X DELETE \
  https://restful-booker.herokuapp.com/booking/1 \
  -H 'Content-Type: application/json' \
  -H 'Cookie: token=abc123'
```

**Пример запроса — Example 2 (Basic auth):**

```bash
curl -X DELETE \
  https://restful-booker.herokuapp.com/booking/1 \
  -H 'Content-Type: application/json' \
  -H 'Authorization: Basic YWRtaW46cGFzc3dvcmQxMjM='
```

**Пример ответа — Response:**

```http
HTTP/1.1 201 Created
```

---

<a id="getbooking"></a>
## `GET /booking/:id` — GetBooking

Returns a specific booking based upon the booking id provided

**Заголовки — Header**

| Поле | Тип | Обязательное | Описание |
|---|---|---|---|
| `Accept` | string | **да** | Sets what format the response body is returned in. Can be application/json or application/xml |

**Параметры — Url Parameter**

| Поле | Тип | Обязательное | Описание |
|---|---|---|---|
| `id` | String | **да** | The id of the booking you would like to retrieve |

**Ответ — Success 200**

| Поле | Тип | Описание |
|---|---|---|
| `firstname` | String | Firstname for the guest who made the booking |
| `lastname` | String | Lastname for the guest who made the booking |
| `totalprice` | Number | The total price for the booking |
| `depositpaid` | Boolean | Whether the deposit has been paid or not |
| `bookingdates` | Object | Sub-object that contains the checkin and checkout dates |
| `bookingdates.checkin` | Date | Date the guest is checking in |
| `bookingdates.checkout` | Date | Date the guest is checking out |
| `additionalneeds` | String | Any other needs the guest has |

**Пример запроса — Example 1 (Get booking):**

```bash
curl -i https://restful-booker.herokuapp.com/booking/1
```

**Пример ответа — JSON Response:**

```http
HTTP/1.1 200 OK

{
    "firstname": "Sally",
    "lastname": "Brown",
    "totalprice": 111,
    "depositpaid": true,
    "bookingdates": {
        "checkin": "2013-02-23",
        "checkout": "2014-10-23"
    },
    "additionalneeds": "Breakfast"
}
```

**Пример ответа — XML Response:**

```http
HTTP/1.1 200 OK

<booking>
    <firstname>Sally</firstname>
    <lastname>Brown</lastname>
    <totalprice>111</totalprice>
    <depositpaid>true</depositpaid>
    <bookingdates>
        <checkin>2013-02-23</checkin>
        <checkout>2014-10-23</checkout>
    </bookingdates>
    <additionalneeds>Breakfast</additionalneeds>
</booking>
```

**Пример ответа — URL Response:**

```http
HTTP/1.1 200 OK

firstname=Jim&lastname=Brown&totalprice=111&depositpaid=true&bookingdates%5Bcheckin%5D=2018-01-01&bookingdates%5Bcheckout%5D=2019-01-01
```

---

<a id="partialupdatebooking"></a>
## `PATCH /booking/:id` — PartialUpdateBooking

Updates a current booking with a partial payload

**Заголовки — Header**

| Поле | Тип | Обязательное | Описание |
|---|---|---|---|
| `Content-Type` | string | **да** | Sets the format of payload you are sending. Can be application/json or text/xml |
| `Accept` | string | **да** | Sets what format the response body is returned in. Can be application/json or application/xml |
| `Cookie` | string | нет | Sets an authorization token to access the PUT endpoint, can be used as an alternative to the Authorization |
| `Authorization` | string | нет | YWRtaW46cGFzc3dvcmQxMjM=]   Basic authorization header to access the PUT endpoint, can be used as an alternative to the Cookie header |

**Параметры — Url Parameter**

| Поле | Тип | Обязательное | Описание |
|---|---|---|---|
| `id` | Number | **да** | ID for the booking you want to update |

**Параметры — Request body**

| Поле | Тип | Обязательное | Описание |
|---|---|---|---|
| `firstname` | String | нет | Firstname for the guest who made the booking |
| `lastname` | String | нет | Lastname for the guest who made the booking |
| `totalprice` | Number | нет | The total price for the booking |
| `depositpaid` | Boolean | нет | Whether the deposit has been paid or not |
| `bookingdates.checkin` | Date | нет | Date the guest is checking in |
| `bookingdates.checkout` | Date | нет | Date the guest is checking out |
| `additionalneeds` | String | нет | Any other needs the guest has |

**Ответ — Success 200**

| Поле | Тип | Описание |
|---|---|---|
| `firstname` | String | Firstname for the guest who made the booking |
| `lastname` | String | Lastname for the guest who made the booking |
| `totalprice` | Number | The total price for the booking |
| `depositpaid` | Boolean | Whether the deposit has been paid or not |
| `bookingdates` | Object | Sub-object that contains the checkin and checkout dates |
| `bookingdates.checkin` | Date | Date the guest is checking in |
| `bookingdates.checkout` | Date | Date the guest is checking out |
| `additionalneeds` | String | Any other needs the guest has |

**Пример запроса — JSON example usage:**

```bash
curl -X PUT \
  https://restful-booker.herokuapp.com/booking/1 \
  -H 'Content-Type: application/json' \
  -H 'Accept: application/json' \
  -H 'Cookie: token=abc123' \
  -d '{
    "firstname" : "James",
    "lastname" : "Brown"
}'
```

**Пример запроса — XML example usage:**

```bash
curl -X PUT \
  https://restful-booker.herokuapp.com/booking/1 \
  -H 'Content-Type: text/xml' \
  -H 'Accept: application/xml' \
  -H 'Authorization: Basic YWRtaW46cGFzc3dvcmQxMjM=' \
  -d '<booking>
    <firstname>James</firstname>
    <lastname>Brown</lastname>
  </booking>'
```

**Пример запроса — URLencoded example usage:**

```bash
curl -X PUT \
  https://restful-booker.herokuapp.com/booking/1 \
  -H 'Content-Type: application/x-www-form-urlencoded' \
  -H 'Accept: application/x-www-form-urlencoded' \
  -H 'Authorization: Basic YWRtaW46cGFzc3dvcmQxMjM=' \
  -d 'firstname=Jim&lastname=Brown'
```

**Пример ответа — JSON Response:**

```http
HTTP/1.1 200 OK

{
    "firstname" : "James",
    "lastname" : "Brown",
    "totalprice" : 111,
    "depositpaid" : true,
    "bookingdates" : {
        "checkin" : "2018-01-01",
        "checkout" : "2019-01-01"
    },
    "additionalneeds" : "Breakfast"
}
```

**Пример ответа — XML Response:**

```http
HTTP/1.1 200 OK

<booking>
    <firstname>James</firstname>
    <lastname>Brown</lastname>
    <totalprice>111</totalprice>
    <depositpaid>true</depositpaid>
    <bookingdates>
      <checkin>2018-01-01</checkin>
      <checkout>2019-01-01</checkout>
    </bookingdates>
    <additionalneeds>Breakfast</additionalneeds>
</booking>
```

**Пример ответа — URL Response:**

```http
HTTP/1.1 200 OK

firstname=Jim&lastname=Brown&totalprice=111&depositpaid=true&bookingdates%5Bcheckin%5D=2018-01-01&bookingdates%5Bcheckout%5D=2019-01-01
```

---

<a id="updatebooking"></a>
## `PUT /booking/:id` — UpdateBooking

Updates a current booking

**Заголовки — Header**

| Поле | Тип | Обязательное | Описание |
|---|---|---|---|
| `Content-Type` | string | **да** | Sets the format of payload you are sending. Can be application/json or text/xml |
| `Accept` | string | **да** | Sets what format the response body is returned in. Can be application/json or application/xml |
| `Cookie` | string | нет | Sets an authorization token to access the PUT endpoint, can be used as an alternative to the Authorization |
| `Authorization` | string | нет | YWRtaW46cGFzc3dvcmQxMjM=]   Basic authorization header to access the PUT endpoint, can be used as an alternative to the Cookie header |

**Параметры — Url Parameter**

| Поле | Тип | Обязательное | Описание |
|---|---|---|---|
| `id` | Number | **да** | ID for the booking you want to update |

**Параметры — Request body**

| Поле | Тип | Обязательное | Описание |
|---|---|---|---|
| `firstname` | String | **да** | Firstname for the guest who made the booking |
| `lastname` | String | **да** | Lastname for the guest who made the booking |
| `totalprice` | Number | **да** | The total price for the booking |
| `depositpaid` | Boolean | **да** | Whether the deposit has been paid or not |
| `bookingdates.checkin` | Date | **да** | Date the guest is checking in |
| `bookingdates.checkout` | Date | **да** | Date the guest is checking out |
| `additionalneeds` | String | **да** | Any other needs the guest has |

**Ответ — Success 200**

| Поле | Тип | Описание |
|---|---|---|
| `firstname` | String | Firstname for the guest who made the booking |
| `lastname` | String | Lastname for the guest who made the booking |
| `totalprice` | Number | The total price for the booking |
| `depositpaid` | Boolean | Whether the deposit has been paid or not |
| `bookingdates` | Object | Sub-object that contains the checkin and checkout dates |
| `bookingdates.checkin` | Date | Date the guest is checking in |
| `bookingdates.checkout` | Date | Date the guest is checking out |
| `additionalneeds` | String | Any other needs the guest has |

**Пример запроса — JSON example usage:**

```bash
curl -X PUT \
  https://restful-booker.herokuapp.com/booking/1 \
  -H 'Content-Type: application/json' \
  -H 'Accept: application/json' \
  -H 'Cookie: token=abc123' \
  -d '{
    "firstname" : "James",
    "lastname" : "Brown",
    "totalprice" : 111,
    "depositpaid" : true,
    "bookingdates" : {
        "checkin" : "2018-01-01",
        "checkout" : "2019-01-01"
    },
    "additionalneeds" : "Breakfast"
}'
```

**Пример запроса — XML example usage:**

```bash
curl -X PUT \
  https://restful-booker.herokuapp.com/booking/1 \
  -H 'Content-Type: text/xml' \
  -H 'Accept: application/xml' \
  -H 'Authorization: Basic YWRtaW46cGFzc3dvcmQxMjM=' \
  -d '<booking>
    <firstname>James</firstname>
    <lastname>Brown</lastname>
    <totalprice>111</totalprice>
    <depositpaid>true</depositpaid>
    <bookingdates>
      <checkin>2018-01-01</checkin>
      <checkout>2019-01-01</checkout>
    </bookingdates>
    <additionalneeds>Breakfast</additionalneeds>
  </booking>'
```

**Пример запроса — URLencoded example usage:**

```bash
curl -X PUT \
  https://restful-booker.herokuapp.com/booking/1 \
  -H 'Content-Type: application/x-www-form-urlencoded' \
  -H 'Accept: application/x-www-form-urlencoded' \
  -H 'Authorization: Basic YWRtaW46cGFzc3dvcmQxMjM=' \
  -d 'firstname=Jim&lastname=Brown&totalprice=111&depositpaid=true&bookingdates%5Bcheckin%5D=2018-01-01&bookingdates%5Bcheckout%5D=2018-01-02'
```

**Пример ответа — JSON Response:**

```http
HTTP/1.1 200 OK

{
    "firstname" : "James",
    "lastname" : "Brown",
    "totalprice" : 111,
    "depositpaid" : true,
    "bookingdates" : {
        "checkin" : "2018-01-01",
        "checkout" : "2019-01-01"
    },
    "additionalneeds" : "Breakfast"
}
```

**Пример ответа — XML Response:**

```http
HTTP/1.1 200 OK

<booking>
    <firstname>James</firstname>
    <lastname>Brown</lastname>
    <totalprice>111</totalprice>
    <depositpaid>true</depositpaid>
    <bookingdates>
      <checkin>2018-01-01</checkin>
      <checkout>2019-01-01</checkout>
    </bookingdates>
    <additionalneeds>Breakfast</additionalneeds>
</booking>
```

**Пример ответа — URL Response:**

```http
HTTP/1.1 200 OK

firstname=Jim&lastname=Brown&totalprice=111&depositpaid=true&bookingdates%5Bcheckin%5D=2018-01-01&bookingdates%5Bcheckout%5D=2019-01-01
```

---

<a id="ping"></a>
## `GET /ping` — HealthCheck

A simple health check endpoint to confirm whether the API is up and running.

**Ответ — Success 200**

| Поле | Тип | Описание |
|---|---|---|
| `OK` | String | Default HTTP 201 response |

**Пример запроса — Ping server:**

```bash
curl -i https://restful-booker.herokuapp.com/ping
```

**Пример ответа — Response:**

```http
HTTP/1.1 201 Created
```

