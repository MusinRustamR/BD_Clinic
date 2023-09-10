1) Напишите запрос по своей базе с inner join.

Получим данные по пациентам,врачам и параметрам приема на основе таблицы "appointment"(записи к врачу)
*****
```
SELECT cl.lastname as 'Фамилия клиента',cl.firstname as'Имя клиента',cl.middlename as 'Отчество клиента', 
emp.lastname as 'Фамилия врача',emp.firstname as 'Имя врача',ap.duration as 'Длительность приема',ap.date as 'Дата', ap.comment as 'Комментарий'
FROM  medclinic.appointment ap
join medclinic.client cl on cl.id=ap.client_Id
join medclinic.employee emp on emp.id=ap.employee_Id
order by emp.lastname 
```
![image](https://github.com/MusinRustamR/BD_Clinic/assets/126672650/a561f7c6-a2f4-4af8-901c-8cee3467df91)

2) Напишите запрос по своей базе с left join
Напишите 5 запросов с WHERE с использованием разных
операторов, опишите для чего вам в проекте нужна такая выборка данных
