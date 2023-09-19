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
