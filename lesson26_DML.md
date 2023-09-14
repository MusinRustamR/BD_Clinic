1)Группировки с ипользованием: 

ROLLUP
```
SELECT emp.lastname as doctor, count(ap.id) 
FROM employee emp
left join  appointment ap on ap.employee_Id=emp.id
group by doctor with rollup ;
```
![image](https://github.com/MusinRustamR/BD_Clinic/assets/126672650/c1a2dc87-eec3-47f6-8044-acdab65424ae)

GROUPING()
```
SELECT IF (Grouping(emp.lastname),'Общее количество приемов', emp.lastname ) as doctor, count(ap.id) 
FROM employee emp
left join  appointment ap on ap.employee_Id=emp.id
group by emp.lastname with rollup ;
```
![image](https://github.com/MusinRustamR/BD_Clinic/assets/126672650/cf2bb92e-be31-49b6-b034-2c0f1f2c5065)


HAVING
```
SELECT emp.lastname as doctor, count(ap.id) as appoint
FROM employee emp
left join  appointment ap on ap.employee_Id=emp.id
group by doctor having appoint > 300 ;
```
![image](https://github.com/MusinRustamR/BD_Clinic/assets/126672650/cb232cf1-ec59-445d-8f53-28bec5610bd8)

CASE
```
with statistics as (
SELECT emp.lastname as doctor, count(ap.id) as appoint
FROM employee emp
left join  appointment ap on ap.employee_Id=emp.id
group by doctor order by appoint
)
select 
case
when appoint <200 then '0-199'
when appoint >=200 and appoint <300 then '200-299'
when appoint >=300 then '300+'
end as appoint_level,
group_concat(doctor)
from statistics
group by 
case
when appoint <200 then '0-199'
when appoint >=200 and appoint <300 then '200-299'
when appoint >=300 then '300+'
end
```
![image](https://github.com/MusinRustamR/BD_Clinic/assets/126672650/1b616feb-8bb8-4873-a00c-04eba9635f17)


2)для магазина к предыдущему списку продуктов добавить максимальную и минимальную цену и кол-во предложений
3)сделать выборку показывающую самый дорогой и самый дешевый товар в каждой категории
4)сделать rollup с количеством товаров по категориям
