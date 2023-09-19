Создадим полнотекстовый индекс для таблицы "Диагнозы" по полям "name" и "description" 
```
alter table diagnosis add fulltext index name_desc_idx (name,description)
```
Проверим использование индекса при запросе 
![image](https://github.com/MusinRustamR/BD_Clinic/assets/126672650/fd2b9d58-f4d1-49f7-a059-96e049df76f6)

Произведем поиск и получим выборку записей, связанных полями "name" или "description" с поисковыми словами.
![image](https://github.com/MusinRustamR/BD_Clinic/assets/126672650/4a1d60c0-5e1c-4c0c-b302-d81c91249942)


