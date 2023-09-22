Рассмотрим запрос 

Стандартный explain
```
explain 
select  ap.date as visit_date, cl.lastname as client_lastname ,cl.firstname as client_firstname , cl.phone as client_phone, ap.isVHI as vhi,sp.name as speciality, emp.lastname as doctor_lastname, dg.name as diagnos
from appointment ap
left join client cl on ap.client_id=cl.id
left join employee emp on ap.employee_id=emp.id
left join employee_speciality es on emp.id=es.employee_id
left join speciality sp on es.speciality_id=sp.id
left join diagnosis dg on ap.diagnosis_id= dg.id 
where sp.name like 'терапевт' and cl.vhi_id=any (select id from vhi where insurer_name like 'reso')
```
![image](https://github.com/MusinRustamR/BD_Clinic/assets/126672650/6d336c51-0b4f-470f-a4ce-0c84307ad396)

В формате tree

![image](https://github.com/MusinRustamR/BD_Clinic/assets/126672650/b3f0f6f3-b135-4cc9-abe6-33db33e97123)

В формате JSON

![image](https://github.com/MusinRustamR/BD_Clinic/assets/126672650/f1dbd3c9-6422-49b2-a88f-d5fe61e011af)

![image](https://github.com/MusinRustamR/BD_Clinic/assets/126672650/3f862281-9fb9-4f30-9487-c32ae4a36f77)

Добавим индексы для name таблицы speciality и insurer_name таблицы vhi

![image](https://github.com/MusinRustamR/BD_Clinic/assets/126672650/b3d6b3d6-69a8-4e09-94dd-c7a8d174e3f1)

Время запроса уменьшилось чуть ~ в 2 раза
![image](https://github.com/MusinRustamR/BD_Clinic/assets/126672650/e0af8616-aaf9-4f6c-87a0-6456aee02e41)



