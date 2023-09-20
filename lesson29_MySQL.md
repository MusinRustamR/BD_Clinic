Напишем хранимую процедуру для выборки данных по фамилии клиента, специальности доктора, виду приема-по полису ДМС или нет, временному интервалу и параметрам сортировки и отбора результатов.
```
CREATE DEFINER=`root`@`%` PROCEDURE `Get_appoitment_on_client`(IN lastname_client varchar(100),  IN speciality_name varchar(100), IN is_vhi bool, IN date_from timestamp,IN date_to timestamp, 
IN order_by_1doc_2diag_3date_appoint int, IN n_limit int, IN n_offset int )
BEGIN
select  ap.date as visit_date, cl.lastname as client_lastname ,cl.firstname as client_firstname , cl.phone as client_phone, ap.isVHI as vhi,sp.name as speciality, emp.lastname as doctor_lastname, dg.name as diagnos
from appointment ap
left join client cl on ap.client_id=cl.id
left join employee emp on ap.employee_id=emp.id
left join employee_speciality es on emp.id=es.employee_id
left join speciality sp on es.speciality_id=sp.id
left join diagnosis dg on ap.diagnosis_id= dg.id 
where cl.lastname=lastname_client and sp.name like speciality_name and ap.isVHI=is_vhi and ap.date between date_from and date_to
order by(case order_by_1doc_2diag_3date_appoint 
		when 1 then doctor_lastname
                when 2 then diagnos
                when 3 then visit_date
                else visit_date
		end )
limit n_limit offset n_offset ;
END
```
Выполним запрос с сортировкой по дате приема и ограничением количества записей=20
![image](https://github.com/MusinRustamR/BD_Clinic/assets/126672650/588b2b81-5611-400d-9158-661e37a45e92)

Попробуем выполнить запрос под пользователем client

![image](https://github.com/MusinRustamR/BD_Clinic/assets/126672650/65a06312-4a26-49b1-82de-b4283e699e05)

Выдадим права на выполнение созданной процедуры и повторим запрос
```
GRANT EXECUTE ON PROCEDURE `medclinic`.`Get_appoitment_on_client` TO 'client'@'%';
```
![image](https://github.com/MusinRustamR/BD_Clinic/assets/126672650/980e4a19-b868-4b50-b3b6-7538e0da65c0)

Напишем хранимую процедуру для выборки списка приемов у врачей за час, день, неделю.
```
CREATE DEFINER=`root`@`%` PROCEDURE `Get_appoitment_report`( IN start_date_time timestamp, IN rep_type_hour1_day2_week3 int)
BEGIN
select  ap.date as visit_date, cl.lastname as client_lastname ,cl.firstname as client_firstname , cl.phone as client_phone, ap.isVHI as vhi,sp.name as speciality, emp.lastname as doctor_lastname, dg.name as diagnos
from appointment ap
left join client cl on ap.client_id=cl.id
left join employee emp on ap.employee_id=emp.id
left join employee_speciality es on emp.id=es.employee_id
left join speciality sp on es.speciality_id=sp.id
left join diagnosis dg on ap.diagnosis_id= dg.id 
where (case rep_type_hour1_day2_week3 
		when 1 then ap.date between start_date_time and date_add( start_date_time, interval 1 hour)
                when 2 then  ap.date between start_date_time and date_add( start_date_time, interval 1 day)
                when 3 then  ap.date between start_date_time and date_add( start_date_time, interval 1 week)
                else start_date_time and date_add( start_date_time, interval 1 day)
		end )
        order by visit_date asc;
END
```
Проверим работу процедуры
![image](https://github.com/MusinRustamR/BD_Clinic/assets/126672650/b0b4d201-070d-4571-bad8-32b2192616e6)

Выполним запрос под пользователем manager, до выдачи прав 

![image](https://github.com/MusinRustamR/BD_Clinic/assets/126672650/23dbf1ba-6a28-42f4-90be-7f2fd6fc7bd3)

и после выдачи 
```
GRANT EXECUTE ON PROCEDURE `medclinic`.`Get_appoitment_report` TO 'manager'@'%';
```

![image](https://github.com/MusinRustamR/BD_Clinic/assets/126672650/55f68824-9ab5-4420-887b-5331220f9e6c)

