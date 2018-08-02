---
title: Магазины, смарт-терминалы и сотрудники
permalink: rest_hierarchy.html
sidebar: evorest
product: REST API
---

В разделе описаны методы для получения таких сущностей пользователей Эвотора, как:

* магазины;
* сотрудники;
* смарт-терминалы.

## Получить список магазинов

    GET /stores

### Параметры запроса

Имя  | Тип  | Описание
-----|------|--------------
`cursor`| `string` | Идентификатор курсора для постраничного чтения данных.

### Ответ

```
200 Успешно
```

```json
{
  "items": [
    {
      "id": "20170228-F4F1-401B-80FA-9ECCA8451FFB",
      "name": "Мой магазин",
      "address": "Россия, г. Москва, ул. Тимура Фрунзе, 24"
    }
  ],
  "paging": {
    "next_cursor": "string"
  }
}
```

## Получить список смарт-терминалов

    GET /devices

### Параметры запроса

Имя  | Тип  | Описание
-----|------|--------------
`cursor`| `string` | Идентификатор курсора для постраничного чтения данных.

### Ответ

```
200 Успешно
```

```json
{
  "items": [
    {
      "id": "20170222-D58C-40E0-8051-B53ADFF38860",
      "name": "Моя касса №1",
      "store_id": "20170228-F4F1-401B-80FA-9ECCA8451FFB",
      "timezone_offset": 10800000,
      "imei": "123456789012345",
      "firmware_version": "1.2.3",
      "location": {
        "lng": 12.34,
        "lat": 12.34
      }
    }
  ],
  "paging": {
    "next_cursor": "string"
  }
}
```

Имя  | Тип  | Описание
-----|------|--------------
`location`| `object` | GPS-координаты смарт-терминала.
`location\lng`| `float` | Долгота.
`location\lat`| `float` | Широта.

## Получить список сотрудников

    GET /employees

### Параметры запроса

Имя  | Тип  | Описание
-----|------|--------------
`cursor`| `string` | Идентификатор курсора для постраничного чтения данных.

### Ответ

```
200 Успешно
```

```json
{
  "items": [
    {
      "id": "20170222-5670-4067-8017-FF5F40F1A23E",
      "name": "Иван",
      "last_name": "Иванов",
      "patronymic_name": "Иванович",
      "phone": 79876543210,
      "stores": [
        {
          "storeUuid": "20170222-d58c-40e0-8051-b53adff38860"
        }
      ],
      "role": "ADMIN"
    }
  ],
  "paging": {
    "next_cursor": "string"
  }
}
```

Имя  | Тип  | Описание
-----|------|--------------
`stores`| `Array of objects` | Массив объектов, которые содержат идентификаторы магазинов, к которым относится сотрудник.
`role`| `strting` | Роль сотрудника, которую пользователь Эвотора указывает в Личном кабинете. Роль определяет права доступа сотрудника к смарт-терминалу.

Возможные значения поля `role`:

* `ADMIN` – предустановленная роль администратора с полным доступом к функциям смарт-терминала.
* `CASHIER` – предустановленная роль кассира с ограниченным доступом к функциям смарт-терминала.
* `MANUAL` – роль, которую пользователь создал в Личном кабинете вручную.