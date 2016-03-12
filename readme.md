# Api сайта rzd.ru

Допустимые запросы через Curl (POST и GET)
Для обхода защиты сайта необходимо предварительно отправить запрос для получения cookies и номера идентификатора RID
Вторым запросом подставляем уникальный идентификатор RID и отправляем cookie

###Ответы с сайта
Статус ответа содержится в переменной result
RID - означает что сайт выдал нам уникальный идентификатор и куки
OK - получен полный ответ с запрошенными нами данными

##Пример запроса
###Первый запрос
https://pass.rzd.ru/timetable/public/ru?STRUCTURE_ID=735&layer_id=5371&dir=0&tfl=3&checkSeats=1&code0={{code_from}}&dt0={{date}}&code1={{code_to}}&dt1={{date}}

###Второй и следующие запросы
https://pass.rzd.ru/timetable/public/ru?STRUCTURE_ID=735&layer_id=5371&rid={{rid}}

Второй запрос выполняется с уже полученным нами уникальным идентификатром который хранит в себе данные предыдущего запроса и куками
Поэтому в целях оптимизации можно не отправлять  некоторые параметры указанные нами в первом запросе


##Реализованные запросы

Необходимо реализовать отдачу данных через ajax-запросы, в текущих примерах это не реализовано

###freeSeats - получает количество свободных мест

Принимает параметры
обязательные параметр при 1 запросе
* STRUCTURE_ID - ID категории (735)
* layer_id - подкатегория (5371)

необязательные параметр при 2 запросе
* dir - 0 только в один конец, 1 - туда-обратно
* tfl - тип поезда (1- все, 2 - дальнего следования, 3- электрички)
* checkSeats поиск в поездах без свободных мест
* code0 - код станции отправления
* code1 - код станции прибытия
* dt0 - дата отправления
* dt1 - дата возвращения

Возвращает массив поездов и свободных мест
* from - название станции отправления (САНКТ-ПЕТЕРБУРГ)
* where - название станции прибытия (КИРОВ ПАСС)
* date -  дата отправления (27.03.2016)
* fromCode - код станции отправления (2004000)
* whereCode - код станции прибытия (2060600)

Массив поездов содержит
* date0 - дата отправления
* date0 - дата прибытия
* time0 - время отправления
* time1 - время прибытия
* varPrice -
* route0 - код станции отправления С-ПЕТ-ЛАД
* route1 - код станции прибития ТЮМЕНЬ
* number - номер поезда
* timeInWay - время в пути

* cars - массив свободных мест купе, плацкарт и люкс
* cars.freeSeats
* cars.itype
* cars.servCls
* cars.tariff - стоимость билета
* cars.pt
* cars.typeLoc - полное наименование (Плацкартный, СВ, Купе, Люкс)
* cars.type - сокращенное наименование (Купе, плац, люкс)

* time0 - время отправления
* tnum0 - номер поезда