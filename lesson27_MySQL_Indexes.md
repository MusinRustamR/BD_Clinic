Создадим полнотекстовый индекс для таблицы "diagnosis" по полям "name" и "description" 
```
alter table diagnosis add fulltext index name_desc_idx (name,description)
```
Проверим использование индекса при запросе 
![image](https://github.com/MusinRustamR/BD_Clinic/assets/126672650/fd2b9d58-f4d1-49f7-a059-96e049df76f6)

Произведем поиск и получим выборку записей, связанных полями "name" или "description" с поисковыми словами.
![image](https://github.com/MusinRustamR/BD_Clinic/assets/126672650/4a1d60c0-5e1c-4c0c-b302-d81c91249942)


Добавим индекс для таблицы "client" по полям "lastname" и "firstname" сравним время выполнения запросов.
```
select * FROM medclinic.client where lastname ='Старшинов' and firstname='Эдуард';
alter table client add index  lfn_idx (lastname,firstname);
select * FROM medclinic.client where lastname ='Старшинов' and firstname='Эдуард';
show profiles;
```
![image](https://github.com/MusinRustamR/BD_Clinic/assets/126672650/bc3d7815-9b9a-43ea-b486-e5c1c8997a7f)

Время запроса уменьшилось ~ в 50 раз
