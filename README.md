 ## База данных Медицинской клиники

 ********
1. Схема БД <image src="/images/схемаБДpg.png" alt="Текст с описанием картинки">
2. Описание 
 
 В разделе 1 представлена схема БД Медицинской клиники.\
 Сущности:
  * Client - таблица посетителей клиники с набором личных данных.
  * Appointment - таблица записей на прием с указанием клиента (ClientID), персонала клиники/врача (EmployeeId), даты приема, продолжительности, поставленного диагноза, комментария по проведенному приему и метки о приеме по полису ДМС.
  * VHI - таблица полисов ДМС клиентов с указанием (ClientID), периодом действия полиса и наименованием страховой компании.
  * Employee - таблица персонала клиники, включающая личные данные персонала, отношение к определенному отделению.
  * Department - таблица отделений клиники, содержит поля наименование отделения, Id главного по отделению, телефон.
  * Speciality - таблица должностей персонала.
  * EmployeeSpeciality - таблица связки персонала и должности.
 
 3. Бизнес-задачи
 
Посредством представленной схемы БД возможно вести учет клиентов клиники, формировать личный кабинет клиента (пациента) с информацией о посещениях и обследованиях, производить онлайн запись на прием, формировать статистику по загруженности отделений и специалистов.
 
 4. Описание применяемых индексов
 
 Таблица "Appointment" - индекс по столбцу "Date" позволит оперативнее собирать данные по пациентам в промежутке времени.\
     _CREATE INDEX date_idx on Appointment(Date);_\
 
 Таблица "Appointment" - составной индекс по столбцам "ClientId" и "EmployeeId" позволит оперативнее выполнить поиск данных по приему с условием определенного сотрудника и пациента.\
     _CREATE INDEX client_employee_id_idx on Appointment(ClientId,EmployeeId);_
 
 Таблица "Client" - составной индекс по столбцам "LastName" и "FirstName" т.к. фамилия имеет большую кардинальность. Обеспечит производительность поиска пациентов.\
    _CRATE INDEX last_first_name_idx on Client(LastName,FirstName)_
 
