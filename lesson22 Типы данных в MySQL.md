Перенесем проект из БД Postgre в MySQL. При написании DDL были добавлены ограничения not null. Типы данных приведены в соответствие с типами MySQL.
*****
```
CREATE TABLE IF NOT EXISTS medclinic.district
(
    id INT PRIMARY KEY AUTO_INCREMENT,
    name varchar(255) 
);

CREATE TABLE IF NOT EXISTS medclinic.street
(
    id INT PRIMARY KEY PRIMARY KEY NOT NULL AUTO_INCREMENT,
    name varchar(255)  NOT NULL,
    district_id integer NOT NULL,
    FOREIGN KEY (district_id)  REFERENCES district (id) ON DELETE RESTRICT ON UPDATE RESTRICT
);

CREATE TABLE IF NOT EXISTS medclinic.client_adress
(
    id INT PRIMARY KEY NOT NULL AUTO_INCREMENT,
    street_id integer NOT NULL,
    house integer NOT NULL,
    house_character varchar(5) ,
    entrace integer,
    flat integer NOT NULL,
    FOREIGN KEY (street_id)  REFERENCES street (id) ON DELETE RESTRICT ON UPDATE RESTRICT        
);

CREATE TABLE IF NOT EXISTS medclinic.vhi
(
    id INT PRIMARY KEY NOT NULL AUTO_INCREMENT,
    cert_numb varchar(100)  NOT NULL,
    insurer_name varchar(255)  NOT NULL,
    startdate timestamp NOT NULL,
    enddate timestamp NOT NULL       
);

CREATE TABLE IF NOT EXISTS medclinic.client
(
    id INT PRIMARY KEY NOT NULL AUTO_INCREMENT,
    lastname varchar(100)  NOT NULL,
    firstname varchar(100)  NOT NULL,
    middlename varchar(100) ,
    phone varchar(12)  NOT NULL,
    adress_id integer NOT NULL,
    birthdate date NOT NULL,
    vhi_id integer NOT NULL,
    FOREIGN KEY (adress_id)  REFERENCES client_adress (id) ON DELETE RESTRICT ON UPDATE RESTRICT,
	FOREIGN KEY (vhi_id)  REFERENCES vhi (id) ON DELETE RESTRICT ON UPDATE RESTRICT    
);


CREATE TABLE IF NOT EXISTS medclinic.department
(
    id INT PRIMARY KEY NOT NULL AUTO_INCREMENT,
    name varchar(255)  NOT NULL,
    head_doctor_id integer NOT NULL
);

CREATE TABLE IF NOT EXISTS medclinic.employee
(
    id INT PRIMARY KEY NOT NULL AUTO_INCREMENT,
    lastname varchar(100)  NOT NULL,
    firstname varchar(100) ,
    middlename varchar(100)  NOT NULL,
    birthdate date NOT NULL,
    phone varchar(12)  NOT NULL,
    email varchar(255) ,
    department_id integer,
	FOREIGN KEY (department_id)  REFERENCES department (id) ON DELETE RESTRICT ON UPDATE RESTRICT      
);

ALTER TABLE  medclinic.department ADD FOREIGN KEY (head_doctor_id)  REFERENCES employee (id) ON DELETE RESTRICT ON UPDATE RESTRICT ;    

CREATE TABLE IF NOT EXISTS medclinic.diagnosis
(
    id INT PRIMARY KEY NOT NULL AUTO_INCREMENT,
    name text  NOT NULL,
    description text 
);

CREATE TABLE IF NOT EXISTS medclinic.treatment_type
(
    id INT PRIMARY KEY NOT NULL AUTO_INCREMENT,
    name varchar(255)  NOT NULL
);

CREATE TABLE IF NOT EXISTS medclinic.treatment
(
    id INT PRIMARY KEY NOT NULL AUTO_INCREMENT,
    name text  NOT NULL,
    cost decimal(10, 2) ,
    treatment_type_id integer NOT NULL,
	FOREIGN KEY (treatment_type_id)  REFERENCES treatment_type (id) ON DELETE RESTRICT ON UPDATE RESTRICT
);

CREATE TABLE IF NOT EXISTS medclinic.speciality
(
    id INT PRIMARY KEY NOT NULL AUTO_INCREMENT,
    name varchar(150)  NOT NULL        
);

CREATE TABLE IF NOT EXISTS medclinic.employee_speciality
(
    id INT PRIMARY KEY NOT NULL AUTO_INCREMENT,
    speciality_id integer NOT NULL,
    employee_id integer NOT NULL ,
    FOREIGN KEY (speciality_id)  REFERENCES speciality (id) ON DELETE RESTRICT ON UPDATE RESTRICT,
    FOREIGN KEY (employee_id)  REFERENCES employee (id) ON DELETE RESTRICT ON UPDATE RESTRICT
);

CREATE TABLE IF NOT EXISTS medclinic.appointment
(
    id INT PRIMARY KEY NOT NULL AUTO_INCREMENT,
    employee_Id integer NOT NULL,
    client_Id integer NOT NULL,
    date timestamp NOT NULL,
    duration time,
    comment text,
    isVHI boolean NOT NULL,
    diagnosis_id integer NOT NULL,
    is_primary boolean NOT NULL,
    FOREIGN KEY (employee_Id)  REFERENCES employee (id) ON DELETE RESTRICT ON UPDATE RESTRICT,
    FOREIGN KEY (client_Id)  REFERENCES client (id) ON DELETE RESTRICT ON UPDATE RESTRICT,
    FOREIGN KEY (diagnosis_id)  REFERENCES diagnosis (id) ON DELETE RESTRICT ON UPDATE RESTRICT
);

CREATE TABLE IF NOT EXISTS medclinic.appointment_treatment
(
    id INT PRIMARY KEY NOT NULL AUTO_INCREMENT,
    appointment_id integer NOT NULL,
    treatment_id integer NOT NULL,
    FOREIGN KEY (appointment_id)  REFERENCES appointment (id) ON DELETE RESTRICT ON UPDATE RESTRICT,
    FOREIGN KEY (treatment_id)  REFERENCES treatment (id) ON DELETE RESTRICT ON UPDATE RESTRICT
);

CREATE TABLE IF NOT EXISTS medclinic.employee_speciality_count
(
    speciality_id integer NOT NULL,
	employee_count integer NOT NULL,
    speciality_name varchar(255),
    specialist text,
    FOREIGN KEY (speciality_id)  REFERENCES speciality (id) ON DELETE RESTRICT ON UPDATE RESTRICT
);
```
Для типа данных JSON создадим таблицу с данными по лабораторным исследованиям. Ввиду того, что результаты исследований могут быть числовым показателем различного порядка, либо булевым типом и т.д., применение типа JSON кажется оправданным. 
```
CREATE TABLE IF NOT EXISTS medclinic.analysis_result
(
    id INT PRIMARY KEY NOT NULL AUTO_INCREMENT,
    client_Id integer NOT NULL,
    treatment_id integer NOT NULL,
    date timestamp NOT NULL,
    result json,
    FOREIGN KEY (client_Id)  REFERENCES client (id) ON DELETE RESTRICT ON UPDATE RESTRICT,
	FOREIGN KEY (treatment_id)  REFERENCES treatment (id) ON DELETE RESTRICT ON UPDATE RESTRICT
);
```
Заполним таблицу тестовыми данными по типу:
```
 INSERT INTO medclinic.analysis_result(client_Id,treatment_id,date,result)
 VALUES(1,1,'2023-06-09 11:00:00','{"Показатель 1, мг/мл": "56*10^(-2)", "Показатель 2": "пол.","Показатель 3": "отриц."}');
````
![image](https://github.com/MusinRustamR/BD_Clinic/assets/126672650/b8ebc77c-7b27-4e3a-a894-81334755b569)

И сделаем выборку ключу поля JSON:
````
SELECT * FROM medclinic.analysis_result where JSON_EXTRACT (result,'$."Показатель 2"') like '"%отр%"';
````
![image](https://github.com/MusinRustamR/BD_Clinic/assets/126672650/64818d9d-4e52-4d78-bba7-126c4653cc80)


