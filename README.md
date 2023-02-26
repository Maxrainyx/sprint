<h2>Проект Виртуальной стажировки по разработке мобильного приложения для ФСТР (Федерация Спортивного Туризма России).</h2>


<h3>С какой проблемой к нам обратился заказчик:</h3>
<a>
    На <a href="https://pereval.online/">сайте ФСТР</a> ведётся база горных перевалов, которая пополняется туристами.
После преодоления очередного перевала, турист заполняет отчёт в формате PDF и отправляет его на электронную почту федерации.
Экспертная группа ФСТР получает эту информацию, верифицирует, а затем вносит её в базу, которая ведётся в Google-таблице.
Весь процесс очень неудобный и долгий и занимает в среднем от 3 до 6 месяцев.
</a>
<h3>Как заказчик хочет решить эту проблему</h3>
<a>

ФСТР заказала студентам SkillFactory разработать мобильное приложение для Android и IOS, которое упростило бы туристам задачу по отправке данных о перевале и сократило время обработки запроса до трёх дней.
Пользоваться мобильным приложением будут туристы. В горах они будут вносить данные о перевале в приложение и отправлять их в ФСТР, как только появится доступ в Интернет.
Модератор из федерации будет верифицировать и вносить в базу данных информацию, полученную от пользователей, а те в свою очередь смогут увидеть в мобильном приложении статус модерации и просматривать базу с объектами, внесёнными другими.
</a>
<h4>Для пользователя в мобильном приложении будут доступны следующие действия:</h4>
- Внесение информации о новом объекте (перевале) в карточку объекта.
- Редактирование в приложении неотправленных на сервер ФСТР данных об объектах. На перевале не всегда работает Интернет.
- Заполнение ФИО и контактных данных (телефон и электронная почта) с последующим их автозаполнением при внесении данных о новых объектах.
- Отправка данных на сервер ФСТР.
- Получение уведомления о статусе отправки (успешно/неуспешно).
- Согласие пользователя с политикой обработки персональных данных в случае нажатия на кнопку «Отправить» при отправке данных на сервер.
<h4>Пользователь с помощью мобильного приложения также будет передавать в ФСТР следующие данные о перевале:</h4>
- координаты перевала и его высота;
- имя пользователя;
- почта и телефон пользователя;
- название перевала;
- несколько фотографий перевала.

<p>Допустимые значения поля status:
	'new' - новая заявка(default-значение);
	'pending' — на рассмотрении;
	'accepted' — заявка принята;
	'rejected' — заявка отклонена.
</p>
<h2>Описание методов API</h2>
<h4>Метод <i>POST</i> /submitData/</h4>
Этот метод принимает JSON в теле запроса с информацией о перевале.
Пример JSON:
```JSON
{
    "beauty_title": "пер.",
    "title": "Дятлова",
    "other_titles": "седл.",
    "connect": "0",
    "add_time": "2023-02-20 00:13:14",
    "user": {
        "email": "mxrainy@gmail.com",
        "name": "Максим",
        "second_name": "Максимов",
        "otc": "Максимович",
        "phone": "88005553535"
    },
    "coordinates": {
        "latitude": "33.3333",
        "longitude": "11.1111",
        "height": "2222"
    },
    "level": {
        "winter": "2А",
        "summer": "1А*",
        "autumn": "1А",
        "spring": "2A*"
    },
    "images": [
        {
            "data": "image1",
            "title": "Основание"
        },
        {
            "data": "image2",
            "title": "Седло"
        },
        {
            "data": "image3",
            "title": "Обрыв"
        }
    ]
}
```
Результатом выполнения метода является JSON-ответ содержащий следующие данные:
'status' — код HTTP, целое число:
    200 — запрос успешно обработан;
    400 — Bad Request (при нехватке полей);
    500 — ошибка при выполнении операции.

'message' — строка:
        Success - отправлено успешно;
	Причина ошибки (если она была);

'id' —  идентификатор, который был присвоен объекту
        при добавлении в базу данных при status=200.
Примеры JSON-ответов:
{ "status": 200, "message": "Success", "id": 42 }
{ "status": 400, "message": "Empty request", "id": null}
{ "status": 500, "message": "Ошибка подключения к базе данных", "id": null}

<h4>Метод <i>GET</i> /submitData/</h4>
Этот метод получает одну запись (перевал) по её id с выведением всей информацию об перевале, в том числе статус модерации.
<h6>Пример JSON: </h6>
```JSON
{
        "id": 8,
        "user": {
            "email": "mxrainy@gmail.com",
            "name": "Максим",
            "second_name": "Максимов",
            "otc": "Максимович",
            "phone": "88005553535"
        },
        "coordinates": {
            "id": 1,
            "latitude": 777.11,
            "longitude": 55.77,
            "height": 1111
        },
        "levels": {
            "id": 2,
            "winter": "1С",
            "summer": "1А*",
            "autumn": "1А",
            "spring": "1С*"
        },
        "images": [
            {
                "id": 3,
                "created": "2023-02-20T10:44:29.555124",
                "title": "Основание",
                "data": "",
                "pass": 8
            },
            {
                "id": 22,
                "created": "2022-12-22T10:46:29.777144",
                "title": "Седло",
                "data": "",
                "pass": 8
            }
        ],
        "created": "2023-02-22T10:41:38.631576",
        "beauty_title": "пер.",
        "title": "дятлова",
        "other_titles": "пере.",
        "connect": "0",
        "add_time": "2023-02-20T01:10:11",
        "status": "new"
},
```
Примеры JSON-ответов:
{ "status": 200, "message": "Success", "id": 42 }
{ "status": 400, "message": "There's no such record", "id": null}

<h4>Метод PATCH /submitData/</h4>
Позволяет отредактировать существующую запись (замена), если она в статусе "new". При этом редактировать можно все поля, кроме ФИО, адреса почты и номера телефона.
В качестве результата изменения приходит ответ содержащий следующие данные:

state:
1 — если успешно удалось отредактировать запись в базе данных.
0 — отредактировать запись не удалось.
message: - сообщение об успешном редактировании при state=1 - сообщение о причине неудачного обновления записи при state=0.
Примеры JSON-ответов:
{ "status": 200, "message": "Success", "state": 1 }
{ "status": 400, "message": "It's not a NEW status of the record", "state": 0}

<h4>Метод <i>GET+email</i> /submitData/?user_email=email</h4>

Возвращает данные всех объектов, отправленных на сервер пользователем с почтой email.
Пример запроса:

GET /submitData/?user_email=maxrainy@agmail.com
