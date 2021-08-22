
# документация botbandit api

## список методов:

1. [init.php](#init) — нужен для получения основной информации об аккаунте на старте приложения. также служит для обработки #-информации (скам);
2. [update-menu.php](#update-menu)   — нужен для получения информации в меню приложения;
3. [get-daily-bonus.php](#get-daily-bonus)   — вызывается, когда пользователь нажимает на кнопку получения ежедневного бонуса;
4. [get-daily-bonus.php](#get-daily-bonus)   — вызывается, когда пользователь заходит в меню скама;

## общие ошибки и то, как следует их обрабатывать

|ошибка|описание|обработка|
|--|--|--|
|code 0 *(security error)*|хэш не прошёл проверку|следует отправить соответствующее сообщение в console.log|
|code 1 *(account not registered)*|аккаунт не зарегистирован|следует показать окно с предложением зарегистрироваться|
|code 2 *(account has been banned)*|аккаунт забанен|следует показать окно с баном|

пример ответа с ошибкой:

    {
		"error": "security error",
		"errno": 0
    }

## методы api

<a name="init"></a>
### init.php

нужен для получения основной информации об аккаунте на старте приложения. также служит для обработки #-информации (скам).

|параметр|описание|
|--|--|
|url|url с которым запустилось приложение **(string)**|
|scammer_id|id скамера, если было передано при старте **(int, optional)**|

пример запроса:

    {
		"url": "https://site.com/?vk_user_id=494075&vk_app_id=6736218&vk_is_app_user=1&vk_are_notifications_enabled=1&vk_language=ru&vk_access_token_settings=&vk_platform=android&sign=t4WeyGcuTMSnKECxnjaBQrPBJgw3xNHzky7NcXyTcgI"
    }

|возвращаемое значение|описание|
|--|--|
|id|vkid пользователя **(int)**|
|money|количество денег у пользователя **(int)**|
|sex|пол пользователя. 1 - парень, 0 - девушка **(int)**|
|last_daily_bonus|может ли получить игрок ежедневный бонус **(bool)**|

пример ответа:

    {
		"id": 656739737,
		"money": 150000,
		"sex": 1,
		"last_daily_bonus": 1625910061
    }

<a name="update-menu"></a>
### update-menu.php

нужен для получения информации в меню приложения.

|параметр|описание|
|--|--|
|url|url с которым запустилось приложение **(string)**|

пример запроса:

    {
		"url": "https://site.com/?vk_user_id=494075&vk_app_id=6736218&vk_is_app_user=1&vk_are_notifications_enabled=1&vk_language=ru&vk_access_token_settings=&vk_platform=android&sign=t4WeyGcuTMSnKECxnjaBQrPBJgw3xNHzky7NcXyTcgI"
    }

|возвращаемое значение|описание|
|--|--|
|id|vkid пользователя **(int)**|
|money|количество денег у пользователя **(int)**|
|last_daily_bonus|unix-время, когда последний раз пользователь забирал ежедневных бонус **(int)**|

пример ответа:

    {
		"id": 656739737,
		"money": 150000,
		"last_daily_bonus": 1625910061
    }

<a name="get-daily-bonus"></a>
### get-daily-bonus.php

вызывается, когда пользователь нажимает на кнопку получения ежедневного бонуса

|параметр|описание|
|--|--|
|url|url с которым запустилось приложение **(string)**|

пример запроса:

    {
		"url": "https://site.com/?vk_user_id=494075&vk_app_id=6736218&vk_is_app_user=1&vk_are_notifications_enabled=1&vk_language=ru&vk_access_token_settings=&vk_platform=android&sign=t4WeyGcuTMSnKECxnjaBQrPBJgw3xNHzky7NcXyTcgI"
    }

|возвращаемое значение|описание|
|--|--|
|id|vkid пользователя **(int)**|
|responce|результат **(string)**|
|last_daily_bonus|unix-время, когда последний раз пользователь забирал ежедневных бонус **(int)**|

варианты **responce**:
 - ok
 - limit have not passed

пример ответа:

    {
		"id": 656739737,
		"responce": "ok",
		"last_daily_bonus": 1625910061
    }

<a name="get-daily-bonus"></a>
### get-scam.php

вызывается, когда пользователь заходит в меню скама.

|параметр|описание|
|--|--|
|url|url с которым запустилось приложение **(string)**|

пример запроса:

    {
		"url": "https://site.com/?vk_user_id=494075&vk_app_id=6736218&vk_is_app_user=1&vk_are_notifications_enabled=1&vk_language=ru&vk_access_token_settings=&vk_platform=android&sign=t4WeyGcuTMSnKECxnjaBQrPBJgw3xNHzky7NcXyTcgI"
    }

|возвращаемое значение|описание|
|--|--|
|id|vkid пользователя **(int)**|
|scam_sum|общая сумма скама **(int)**|
|mamonts_count|количество мамонтов **(int)**|
|mamonts|массив мамонтов **(array)**|
|top|массив топ-скамеров **(array)**|


описание элементов массива **mamonts**:
|поле|описание|
|--|--|
|id|vkid мамонта **(int)**|
|type|тип описания скама **(int)**|
|sum|сумма скама **(int)**|

описание элементов массива **top**:
|поле|описание|
|--|--|
|id|vkid мамонта **(int)**|
|top|место в топе **(int)**|
|sum|общая сумма скама пользователя **(int)**|

пример ответа:

    {
		"id": 656739737,
		"scam_sum": 10200000,
		"mamonts_count": 2,
		"mamonts": 
		[
			{
				"id": 1000001,
				"type": 1,
				"sum": 5200000
			},
			{
				"id": 1000002,
				"type": 2,
				"sum": 5000000
			}
		],
		"top":
		[
			{
				"id": 1000010,
				"top": 1,
				"sum": 300000000000
			},
			{
				"id": 1000011,
				"top": 2,
				"sum": 200000000000
			},
			{
				"id": 1000012,
				"top": 3,
				"sum": 100000000000
			}
		]
    }
