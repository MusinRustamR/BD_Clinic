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

2) Напишите запрос по своей базе с left join.
   
   Выполним left join к таблице "Клиенты". Часть полей "Дата" и "Диагноз" имеют null значения т.к. не у всех клиентов были записи к врачу и диагнозы.
```
SELECT cl.lastname as 'Фамилия клиента',cl.firstname as'Имя клиента',cl.middlename as 'Отчество клиента',ap.date as 'Дата', dg.name as 'Диагноз' FROM medclinic.client cl
left join medclinic.appointment ap  on cl.id=ap.client_Id
left join medclinic.diagnosis dg on dg.id=ap.diagnosis_id
```
![image](https://github.com/MusinRustamR/BD_Clinic/assets/126672650/19e2ec32-0e47-4e01-9fd3-76952118f710)


3) Напишите 5 запросов с WHERE с использованием разных операторов, опишите для чего вам в проекте нужна такая выборка данных

   Сделаем выборку пациентов с  определенным диагнозом на определенную дату.
```
SELECT cl.lastname as 'Фамилия клиента',cl.firstname as'Имя клиента',cl.middlename as 'Отчество клиента', 
emp.lastname as 'Фамилия врача',emp.firstname as 'Имя врача',ap.date as 'Дата', ap.comment as 'Комментарий', dg.name as 'Диагноз'
FROM  medclinic.appointment ap
left join medclinic.client cl on cl.id=ap.client_Id
left join medclinic.employee emp on emp.id=ap.employee_id
left join medclinic.diagnosis dg on dg.id=ap.diagnosis_id
where dg.name='Грипп' and ap.date between '2023-04-05 00:00:00' and '2023-04-05 18:00:00';
```
   ![image](https://github.com/MusinRustamR/BD_Clinic/assets/126672650/fd03038c-7698-48f7-8e27-75e920e3eec0)

   Сделаем выборку пациентов по схожим диагнозам с помощью like
```
SELECT cl.lastname as 'Фамилия клиента',cl.firstname as'Имя клиента',cl.middlename as 'Отчество клиента', 
emp.lastname as 'Фамилия врача',emp.firstname as 'Имя врача',ap.date as 'Дата', ap.comment as 'Комментарий', dg.name as 'Диагноз'
FROM  medclinic.appointment ap
left join medclinic.client cl on cl.id=ap.client_Id
left join medclinic.employee emp on emp.id=ap.employee_id
left join medclinic.diagnosis dg on dg.id=ap.diagnosis_id
where dg.name like '%аллергия%' ;
```
![image](https://github.com/MusinRustamR/BD_Clinic/assets/126672650/8bf9e56b-d042-4e52-aa5e-0680ec666add)

Сделаем выборку пациентов по нескольким диагнозам с помощью оператора "in"
```
SELECT cl.lastname as 'Фамилия клиента',cl.firstname as'Имя клиента',cl.middlename as 'Отчество клиента', 
emp.lastname as 'Фамилия врача',emp.firstname as 'Имя врача',ap.date as 'Дата', ap.comment as 'Комментарий', dg.name as 'Диагноз'
FROM  medclinic.appointment ap
left join medclinic.client cl on cl.id=ap.client_Id
left join medclinic.employee emp on emp.id=ap.employee_id
left join medclinic.diagnosis dg on dg.id=ap.diagnosis_id
where dg.name in ('Грипп','Воспаление легких') ;
```

![image](https://github.com/MusinRustamR/BD_Clinic/assets/126672650/c7665084-0a24-4f34-815f-8e8b29eaf7c9)

Проследим динамику заболеваемости после определенной даты.
```
SELECT count( dg.name) as 'Диагноз грипп, кол-во', date( ap.date) as 'Дата' 
FROM  medclinic.appointment ap
left join medclinic.diagnosis dg on dg.id=ap.diagnosis_id
where dg.name ='Грипп' and ap.date > '2023-04-01 00:00:00'
group by  2 order by 2 asc;
```
![image](https://github.com/MusinRustamR/BD_Clinic/assets/126672650/a075351e-dbfa-4e2f-b5c9-489bc6d8f388)


