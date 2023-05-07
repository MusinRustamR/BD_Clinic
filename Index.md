*Создание индекса*

Проведем поиск в таблице клиентов по имени "Никита"\
![image](https://user-images.githubusercontent.com/126672650/236681553-0406232f-a9d2-487f-bc95-900b3e01da8d.png)

Создадим индекс на поле "firstname" и повторим запрос\
![image](https://user-images.githubusercontent.com/126672650/236681667-ce3e06da-495f-4c04-994b-ae0cd33bd186.png)

Время выполенния запроса уменьшилось ~ в 5 раз.

*Индекс для полнотекстового поиска*

Проведем полнотекстовый поиск по таблице "Диагнозы"\
![image](https://user-images.githubusercontent.com/126672650/236700398-92ea7d48-8f56-46f2-9d5c-9f671d8a80d1.png)

Создадим индекс "desc_idx" по столбцу типа tsvector и повторим запрос.\
![image](https://user-images.githubusercontent.com/126672650/236700675-b7911fd7-4d4a-4bdd-8d7b-cc829ebdca5a.png)

Время выполенния запроса изменилось ~ в 100 раз.

*Создание индекса на часть таблицы*

Выполним запрос в таблице "Приёмы" за промежуток времени.\
![image](https://user-images.githubusercontent.com/126672650/236701555-73cf5a66-b6d8-4f01-9a6f-5b0f72968693.png)

Создадим индекс "appoitment_date_idx" с условием, что приемы велись по ДМС. Повторим запрос.


