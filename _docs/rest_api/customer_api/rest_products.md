---
title: Товары и модификации товаров
permalink: rest_products.html
sidebar: evorest
product: REST API
---



## Создать товар(ы)

Создает новый товар или модификацию товара в магазине. Идентификаторы объектов формирует Облако.

Можно создать как один элемент за раз, так и несколько (до 1000).

Для выполнения множественных операций - указывайте Content-Type с модификатором +bulk.

    POST /stores/{store-id}/products

### Параметры запроса

Имя  | Тип  | Описание
-----|------|--------------
`cursor`| `string` | Идентификатор курсора для постраничного чтения данных.
``| `` |  
``| `` |  

### Пример тела запроса

```json
[
  {
    "parent_id": "1ddea16b-971b-dee5-3798-1b29a7aa2e27",
    "name": "Сидр",
    "measure_name": "шт",
    "tax": "VAT_18",
    "allow_to_sell": true,
    "price": 123.12,
    "description": "Вкусный яблочный сидр",
    "article_number": "СДР-ЯБЛЧ",
    "code": "42",
    "barcodes": [
      "2000000000060"
    ],
    "type": "ALCOHOL_NOT_MARKED"
  }
]
```

### Ответ

```
200 Успешно
```

```json

```

```
202 Успешно
```

```json
{
  "id": "ca187ddc-8d1b-4d0e-b20d-c509082da528",
  "modified_at": "2018-01-01T00:00:00.000Z",
  "status": "FAILED",
  "type": "product",
  "details": [
    {
      "message": "validation failed for specified payload",
      "code": "validation_failed",
      "product_id": "1009017d-5f3d-422a-964b-3ff11ee2d183",
      "violations": [
        {
          "reason": "must not be null",
          "subject": "name"
        }
      ]
    }
  ]
}
```

## Создать/заменить несколько товаров

Создает или заменяет товары или модификации товаров в магазине. Идентификаторы объектов формирует клиент API.

Можно передать за раз до 1000 элементов.

    PUT /stores/{store-id}/products

### Параметры запроса

Имя  | Тип  | Описание
-----|------|--------------
`cursor`| `string` | Идентификатор курсора для постраничного чтения данных.
``| `` |  
``| `` |  

### Пример тела запроса

```json
[
  {
    "parent_id": "1ddea16b-971b-dee5-3798-1b29a7aa2e27",
    "name": "Сидр",
    "measure_name": "шт",
    "tax": "VAT_18",
    "allow_to_sell": true,
    "price": 123.12,
    "description": "Вкусный яблочный сидр",
    "article_number": "СДР-ЯБЛЧ",
    "code": "42",
    "barcodes": [
      "2000000000060"
    ],
    "type": "ALCOHOL_NOT_MARKED"
  }
]
```

### Ответ

```
202 Успешно
```

```json
{
  "id": "ca187ddc-8d1b-4d0e-b20d-c509082da528",
  "modified_at": "2018-01-01T00:00:00.000Z",
  "status": "FAILED",
  "type": "product",
  "details": [
    {
      "message": "validation failed for specified payload",
      "code": "validation_failed",
      "product_id": "1009017d-5f3d-422a-964b-3ff11ee2d183",
      "violations": [
        {
          "reason": "must not be null",
          "subject": "name"
        }
      ]
    }
  ]
}
```

## Получить все товары

    GET /stores/{store-id}/products

### Параметры запроса

Имя  | Тип  | Описание
-----|------|--------------
`since` | `long` | Фильтр по дате изменения товара (`updated_at`). Полезен, если полная синхронизация уже была произведена ранее и требуется получить только измененные с конкретного момента времени экземпляры товаров. Указывается только в первом запросе из серии постраничных запросов. Если требуется получить полный список товаров, - не указывайте данный параметр.
`cursor`| `string` | Идентификатор курсора для постраничного чтения данных.

### Ответ

```
200 Success
```

```json
{
  "items": [
    {
      "parent_id": "1ddea16b-971b-dee5-3798-1b29a7aa2e27",
      "name": "Сидр",
      "measure_name": "шт",
      "tax": "VAT_18",
      "allow_to_sell": true,
      "price": 123.12,
      "description": "Вкусный яблочный сидр",
      "article_number": "СДР-ЯБЛЧ",
      "code": "42",
      "barcodes": [
        "2000000000060"
      ],
      "type": "ALCOHOL_NOT_MARKED"
    }
  ],
  "paging": {
    "next_cursor": "string"
  }
}
```

## Удалить несколько товаров или модификаций товаров

Удаляет товары и модификации товаров из магазина.

Чтобы удалить несколько элементов, укажите через запятую идентификаторы элементов к удалению в параметре `id`.

Можно удалить до 100 элементов за один запрос.

    DELETE /stores/{store-id}/products

### Параметры запроса

Имя  | Тип  | Описание
-----|------|--------------
`id`| `string` | Идентификатор товара в Облаке Эвотор.

### Ответ

```
204 Успешно
```


## Создать/заменить товар

    PUT /stores/{store-id}/products/{product-id}


### Параметры запроса

Имя  | Тип  | Описание
-----|------|--------------
`cursor`| `string` | Идентификатор курсора для постраничного чтения данных.
``| `` |  
``| `` |  

### Пример тела запроса

```json
{

}
```

### Ответ

```
200 Успешно
```

## Получить товар

    GET /stores/{store-id}/products/{product-id}

### Ответ

```
200 Успешно
```


## Обновить остатки товара

    PATCH /stores/{store-id}/products/{product-id}

### Параметры запроса

Имя  | Тип  | Описание
-----|------|--------------
`cursor`| `string` | Идентификатор курсора для постраничного чтения данных.
`quantity`| `number` |  
`price`| `number` |  

### Пример тела запроса

```json
{
  "quantity": 12,
  "price": 123.12
}
```

### Ответ

```
200 Успешно
```

## Удалить товар

    DELETE /stores/{store-id}/products/{product-id}

### Ответ

```
204 Успешно
```