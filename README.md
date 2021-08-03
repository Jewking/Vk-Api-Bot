### Vk-Api-Bot
###### Simple bot for VK.com (Spring Boot / Log4j / Lombok)

# 
##### Описание проекта

<p align="center">
  <img src="https://i.imgur.com/kNShcE2.png" />
</p>

:star: Это простенький бот для VK, который может принимать, обрабатывать сообщения :bell: и отправлять ответ. 
Логируются все отправляемые и принимаемые уведомления, в том числе и приходяшие ошибки. 
+ По умолчанию: логи :scroll: записываются в консоль, чтобы логи записывались в файл необходимо раскомментировать конфигурацию в ___log4j.propetyies___.
+ Был создан абстрактный класс Response, от которого можно наследоваться и создавать пользовательские сценарии ответа. Масштабируемость - наше всё. По-хорошему есть смысл добавить словарь и - _класс анализатор сообщения пользователя_ или _давать готовый список для выбора уже захардкоденых "команд"_. Смысла добавлять и усложнять существующую уже структуру я не видел, исходя из поствленной задачи, поэтому оставил _поле_ для творчества 🎨.

##### В DEBUG режиме:
```
DEBUG	2021-08-02 23:41:41,864	1	com.tasks.test.vkapibot.service.VkApiService	[http-nio-9999-exec-1]	Received: type: 'MESSAGE_NEW', body: 'Hello', from user_id: 17213748
DEBUG	2021-08-02 23:41:42,478	615	com.tasks.test.vkapibot.response.Response	[http-nio-9999-exec-1]	GET https://api.vk.com/method/messages.send?message=%D0%92%D1%8B+%D0%BD%D0%B0%D0%BF%D0%B8%D1%81%D0%B0%D0%BB%D0%B8%3A+Hello&peer_id=17213748&access_token=d38dadbd481a828a21a253e5ef85ab5a...7dff903593&v=5.89&random_id=-54874627 HTTP/1.1
DEBUG	2021-08-02 23:41:42,484	621	com.tasks.test.vkapibot.response.Response	[http-nio-9999-exec-1]	Received: {"response":91}
DEBUG	2021-08-02 23:41:42,707	844	com.tasks.test.vkapibot.service.VkApiService	[http-nio-9999-exec-2]	Received: type: 'MESSAGE_REPLY', body: 'Вы написали: Hello', to user_id: 17213748
```

##### Полученная ошибка от сервера на отправленное ботом сообщение:
```
ERROR	2021-08-02 23:28:05,074	678	com.tasks.test.vkapibot.response.Response	[http-nio-9999-exec-1]	Received an error: 'User authorization failed: invalid access_token (4).' with code [5]
The following request parameters were passed:
[ {
  "key" : "peer_id",
  "value" : "1713748"
}, {
  "key" : "v",
  "value" : "5.89"
}, {
  "key" : "random_id",
  "value" : "1750887011"
}, {
  "key" : "method",
  "value" : "messages.send"
}, {
  "key" : "oauth",
  "value" : "1"
} ]
```

#
##### Структура проекта
```.
├── ...
├── src/main
│    ├── java
│    │    └── com/tasks/test/vkapibot
│    │            ├── controller             
│    │            │    └── VkApiController.java
│    │            ├── entities
│    │            │    ├── Event
│    │            │    └── EventObject
│    │            ├── enums
│    │            │    ├── ApiCallback
│    │            │    └── ApiMethod
│    │            ├── response
│    │            │    ├── DefaultResponse
│    │            │    └── Response 
│    │            ├── service    
│    │            │    └── VkApiService
│    │            ├── AppConfig.java
│    │            ├── Constatns.java            
│    │            └── VkBotApplication.java       
│    └── resources
│         ├── application.yaml                      
│         └── ...
└── ...
```

#
##### Подготовка к запуску
Для начала необходимо зайти в настройки паблика __Сообщения > Настройки для бота__ и включить возможности ботов :white_check_mark:. После чего перейти в __Настройки > Работа с API__ и создать ключ доступа.

<p align="center">
  <img src="https://i.imgur.com/bXMXKz4.png" />
</p>

Далее перейти в Callback API и добавить сервер. Теперь можно перейти к конфигурации.

#
##### Конфигурация
В __application.yaml__ необходимо указать:
+ Строка, которую должен вернуть сервер при добавлении сервера
+ Ключ доступа
+ Секретный ключ (_Позволит достоверно определять, что уведомление пришло именно от vk_)

```yml
token:
  confirmation: ${your.confirmation.token}
  access: ${your.access.token}

secret:
  key: ${your.access.token}

server:
  port: ${your.server.port}
```


