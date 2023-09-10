Напишите запрос по своей базе с inner join

SELECT cl.lastname as 'Фамилия клиента',cl.firstname as'Имя клиента',cl.middlename as 'Отчество клиента', 
emp.lastname as 'Фамилия врача',emp.firstname as 'Имя врача',ap.duration as 'Длительность приема',ap.comment as 'Комментарий' FROM  medclinic.appointment ap
join medclinic.client cl on cl.id=ap.client_Id
join medclinic.employee emp on emp.id=ap.employee_Id
order by ap.client_Id
Напишите запрос по своей базе с left join
Напишите 5 запросов с WHERE с использованием разных
операторов, опишите для чего вам в проекте нужна такая выборка данных
