# документация botbandit api

## список методов:

1. [init.php](#init) — нужен для получения основной информации об аккаунте на старте приложения. также служит для обработки #-информации (скам);
2. [update-menu.php](#update-menu)   — нужен для получения информации в меню приложения;

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
|daily_bonus|может ли получить игрок ежедневный бонус **(bool)**|

пример ответа:

    {
		"id": 656739737,
		"money": 150000,
		"sex": 1,
		"last_bonus": 1625910061
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
		"last_bonus": 1625910061
    }

