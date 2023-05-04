1.Напишите запрос по своей базе с регулярным выражением, добавьте пояснение, что вы хотите найти.
   
Произвести быстрый поиск клиентов с именем "Александр"
   
*SELECT id, firstname, lastname, middlename, phone, adress_id, birthdate, vhi_id\
FROM client_data.client where "firstname" like 'Алек%'*

      
Найти всех клиентов 1980 года рождения 
    
*SELECT id, firstname, lastname, middlename, phone, adress_id, birthdate, vhi_id\
FROM client_data.client where "birthdate"::text like '1980-%'*

      
2.Напишите запрос по своей базе с использованием LEFT JOIN и INNER JOIN, как порядок соединений в FROM влияет на результат? Почему?
3.Напишите запрос на добавление данных с выводом информации о добавленных строках.
4.Напишите запрос с обновлением данные используя UPDATE FROM.
5.Напишите запрос для удаления данных с оператором DELETE используя join с другой таблицей с помощью using.
