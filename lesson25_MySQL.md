1)Описать пример транзакции из своего проекта с изменением данных в нескольких таблицах. Реализовать в виде хранимой процедуры.
Добавим нового клиента, а также сведения о его медицинском полисе.
```
CREATE DEFINER=`root`@`%` PROCEDURE `add_client`(IN lastname_l varchar(100), IN firstname_f varchar(100), IN middlename_m varchar(100), 
IN phone_n varchar(12), IN birthdate_b date,IN cert_numb_c varchar(100),IN insurer_name_n varchar(255), IN startdate_s timestamp ,IN enddate_e timestamp )
BEGIN
START TRANSACTION;
INSERT INTO  `medclinic`.`vhi`(cert_numb,insurer_name,startdate,enddate) VALUES (cert_numb_c,insurer_name_n,startdate_s,enddate_e);
set @vhiid=last_insert_id();
INSERT INTO  `medclinic`.`client` (lastname,firstname,middlename,phone,birthdate,vhi_id) VALUES (lastname_l,firstname_f,middlename_m,phone_n,birthdate_b, @vhiid);
IF ((SELECT COUNT(*) FROM `medclinic`.`client` WHERE phone = phone_n) > 1) THEN
ROLLBACK;
SELECT 'Phone number exists';
ELSE
COMMIT;
Select last_insert_id();
END IF;
END
```
Выполним процедуру и получим id созданного пользователя, в случае если его тел. номер не существует таблице клиентов.
![image](https://github.com/MusinRustamR/BD_Clinic/assets/126672650/d56e1fe2-a3a6-46be-9f18-18d77b7fa06a)

Проверим созданные записи в таблицах "Клиенты" и "Медицинские полисы"
![image](https://github.com/MusinRustamR/BD_Clinic/assets/126672650/1d2e9168-ce09-4c89-8fb0-7829b777daaa)
![image](https://github.com/MusinRustamR/BD_Clinic/assets/126672650/1d475272-6562-4381-b964-5a4233173f17)

2)Загрузить данные из приложенных в материалах csv.

Загрузим данные из Bicycles.csv
![image](https://github.com/MusinRustamR/BD_Clinic/assets/126672650/9a59d1d3-f010-4663-85a1-e00c04995560)

Результаты загрузки
![image](https://github.com/MusinRustamR/BD_Clinic/assets/126672650/0b720d55-90ec-468a-a2c8-5af19303d4b4)
