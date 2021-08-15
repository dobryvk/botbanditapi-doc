# документация botbandit api

## список методов:

1. [init.php](#init) — нужен для получения основной информации об аккаунте на старте приложения. также служит для обработки #-информации (скам);
2. [update-menu.php](#update-menu)   — нужен для получения информации в меню приложения;
3. [update-token.php](#update-token)   — нужен для обновления токена в базе данных;

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
|token|токен пользователя, если есть. если нету — вернёт NULL. **(string/null)**|

**ВАЖНО!** если токена нет, нужно сразу запросить его и отправить в метод [update-token.php](#update-token). также если в процессе какой-либо операции окажется, что токен не действителен, нужно запросить его и отправить в метод [update-token.php](#update-token).

пример ответа:

    {
	    "id": 656739737,
    	"money": 150000,
    	"sex": 1,
    	"token": "n90u3b8rf9876c54x7rc68tvybo9u8yg7tf6rcd5xecuvyiuboinjuy7tivbghonipoyuvitcyurf4ubyi7tv"
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

пример ответа:

    {
	    "id": 656739737,
    	"money": 150000
    }


<a name="update-token"></a>
### update-token.php

нужен для обновления токена в базе данных. если пользователь не даёт токен, нужно требовать, пока не даст.

|параметр|описание|
|--|--|
|url|url с которым запустилось приложение **(string)**|
|token|access_token полученный через VK Bridge **(string)**|

пример запроса:

    {
    	"url": "https://site.com/?vk_user_id=494075&vk_app_id=6736218&vk_is_app_user=1&vk_are_notifications_enabled=1&vk_language=ru&vk_access_token_settings=&vk_platform=android&sign=t4WeyGcuTMSnKECxnjaBQrPBJgw3xNHzky7NcXyTcgI",
    	"token": "n90u3b8rf9876c54x7rc68tvybo9u8yg7tf6rcd5xecuvyiuboinjuy7tivbghonipoyuvitcyurf4ubyi7tv"
    }

|возвращаемое значение|описание|
|--|--|
|id|vkid пользователя **(int)**|
|responce|ok **(string)**|

пример ответа:

    {
	    "id": 656739737,
		"responce": "ok"
    }

|ошибка|описание|обработка|
|--|--|--|
|code 3001 *(token is required)*|неправильный запрос: нет параметра **token**|следует отправить соответствующее сообщение в console.log|
|code 3002 *(token is invalid)*|неправильный запрос: параметр **token** указан неправильно|следует отправить соответствующее сообщение в console.log|







