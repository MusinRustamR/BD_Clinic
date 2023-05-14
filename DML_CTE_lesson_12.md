*DML: агрегация и сортировка, CTE, аналитические функции*

Создадим таблицу согласно дз

![image](https://github.com/MusinRustamR/BD_Clinic/assets/126672650/19c793c2-06b2-46be-bd10-6446edbbeacc)

1)Написать запрос суммы очков с группировкой и сортировкой по годам

![image](https://github.com/MusinRustamR/BD_Clinic/assets/126672650/839ac315-84e9-43b2-82b4-07897e353190)

2)Написать cte показывающее тоже самое

![image](https://github.com/MusinRustamR/BD_Clinic/assets/126672650/3543d03a-d5f9-4236-b687-8624f4b3ae6a)

3)Используя функцию LAG вывести кол-во очков по всем игрокам за текущий год и за предыдущий.
Выполним запрос за 2019 год. Получаем очки за текущий и предыдущий год у всех, кроме Luke - null pf 2018 год.
![image](https://github.com/MusinRustamR/BD_Clinic/assets/126672650/6ad550d3-7045-495b-9a39-ce13c536264c)
Добавим данные для Mike за 2017 год - points=22 и повторим запрос за 2017 год\
![image](https://github.com/MusinRustamR/BD_Clinic/assets/126672650/6740e363-b4a1-46fb-9af2-27cd6c9b9fce)



