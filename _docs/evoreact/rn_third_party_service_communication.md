---
title: Обмен сообщениями со сторонним сервисом
keywords:
sidebar: evoreact
permalink: rn_third_party_service_communication.html
tags:
---

Все запросы приложений к стороннему сервису проходят через Облако Эвотор:

{: .center-image}
![](images\cloud_proxy.png)

Для отправки запросов приложения могут использовать только 80 и 443 порт.

Смарт-терминал не поддерживает протокол WebSocket.

{% include important.html content="Установленные на смарт-терминале приложения могут обращаться только к сторонним сервисам. При этом, сторонние сервисы не должны быть в одной локальной сети со смарт-терминалом. Обращаться к REST API Облака Эвотор нельзя." %}

## Подготовка к отправке запросов

Для отправки запросов требуется:

* На портале dev.evotor.ru, [указать список разрешённых URL](rn_third_party_service_communication.html#urls), к которым может обращаться приложение.
* Поддержать возможность обращения к сторонним сервисам в приложении.
* [Установить приложение в Магазине приложений](./doc_app_installation.html#MarkeplaceAppInstallation).

  В данном случае установка подразумевает активацию приложения в [Личном кабинете](https://lk.evotor.ru) и необходима, чтобы применились URL, которые вы указали на сайте разработчиков.

{% include note.html content="Если вы разрабатываете Java-приложение, смотрите раздел [\"Обмен сообщениями Java-приложения и стороннего сервиса\"](./doc_java_third_party_service_communication.html)." %}

## Настройка списка разрешённых URL {#urls}

Чтобы настроить список разрешённых URL:

1. На портале [dev.evotor.ru](https://dev.evotor.ru) выберите приложение, для которого требуется указать список разрешённых URL.
2. На вкладке **Интеграция**, установите флажок **Проксирование запросов из приложения с терминала на ваш сервер (ver. 2)** и укажите список URL, к которым может обращаться приложение.

Примеры:

* `http://example\.com/document.*\.jsp\?wildcard=\*&param=value.*`;
* `https://another\.host\.com/document.*\.jsp`;
* `https://another\.host\.com/.*`.

{% include tip.html content="Чтобы задавать маски веб-сайтов используйте [регулярные выражения](http://docs.oracle.com/javase/8/docs/api/java/util/regex/Pattern.html#sum)." %}

После создания списка и [установки приложения в Магазине приложений](./doc_app_installation.html#MarkeplaceAppInstallation), обмен сообщениями происходит по описанному ниже процессу.

## Шаг 1. Приложение отправляет HTTP-сообщение

Приложение создаёт HTTP-сообщение и отправляет его в сторонний сервис. Вы можете воспользоваться любым удобным способом отправки сообщения.

Независимо от версии проксирования SDK смарт-терминала и Облако Эвотор поддерживают следующие HTTP-методы:

* GET;
* POST;
* PUT;
* DELETE.

Эвотор гарантирует поддержку следующих MIME-типов:

* `application/x-www-form-urlencoded`
* `application/json`
* `application/xml`
* `text/*`
* `image/*`
* `multipart/form-data`

Поддержка других MIME-типов не гарантируется.

## Шаг 2. Смарт-терминал передаёт сообщение в облако

Терминал перехватывает сообщение и передаёт его в облако Эвотор.

Заголовки, которые терминал добавляет к запросу вашего приложения:

* `X-Device-ID`
* `X-Device-IMEI`
* `X-User-ID`
* `X-Signed-Url`
* `X-Shop-UUID`
* `X-Device-Salt`
* `X-Device-Algorithm`
* `X-Device-Date`
* `X-Secret-Device-ID`
* `X-OAuth-Client-ID`
* `X-Device-UUID`
* `X-Evotor-*`
* `Expect`
* `Host`
* `Transfer-Encoding`

{% include note.html content="Убедитесь, что ваше приложение не использует перечисленные заголовки. В противном случае, смарт-терминал перезапишет их содержимое." %}

## Шаг 3. Облако передаёт сообщение адресату

Облако удаляет служебные заголовки, добавляет свои заголовки и передаёт сообщение адресату.

Заголовки, которые добавляет облако:

* `X-Evotor-Store-Uuid` – содержит идентификатор магазина в формате uuid4, к которому привязан терминал.
* `X-Evotor-Device-UUID` – содержит идентификатор устройства в формате uuid4, полученный по запросу к `/api/v1/inventories/devices`.
* `X-Evotor-User-Id` – содержит идентификатор пользователя в Облаке Эвотор.
* `Authorization` – содержит [токен пользователя приложения стороннего сервиса](./doc_evotor_api_authorization.html#usersToken). Токен необходим для bearer-авторизации.

Сторонний сервер получает сообщение от облака Эвотор и определяет отправителя с помощью заголовка Authorization.

## Шаг 4. Ответ стороннего сервиса

Ответ стороннего сервиса передаётся приложению в обратном порядке.

Если вы используете проксирование ver.2 требования к содержимому ответа отсутствуют.

Если вы используете проксирование ver.1 ответы стороннего сервиса должны содержать объекты:

  * `status` – содержит HTTP-код состояния.
  * `body` – содержит тело запроса.

{% include tip.html content="За проксирование отвечает параметр **Проксирование запросов из приложения с терминала на ваш сервер** на вкладке **Интеграция** вашего приложения, на сайте [dev.evotor.ru](https://dev.evotor.ru/)." %}