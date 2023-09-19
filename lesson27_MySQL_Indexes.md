Создадим полнотекстовый индекс для таблицы "Диагнозы" по полям "name" и "description" 
```
alter table diagnosis add fulltext index name_desc_idx (name,description)
```
