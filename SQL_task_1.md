Employees
| EmployeeID | FirstName | LastName | ManagerID | Salary | JobPosition     | Active |
| ---------- | --------- | -------- | --------- | ------ | --------------- | ------ |
| 123        | John      | Smith    |       147 |   2000 | QA Engineer     | 0      |
| 456        | Victoria  | Mush     |       147 |   3000 | Senior Engineer | 1      |
| 789        | Harry     | Thomason |       258 |   1500 | QA Engineer     | 1      |

Managers
|ManagerID	| FirstName |	LastName  |
| --------- | --------- | --------- |
|147      	| Maria     | Ferrero   |
|258      	| Jack    	| Henderson |
|369      	| Bob       |	Park      |

1.	Посчитать количество всех сотрудников, которые до сих пор работают в компании (флаг Active).
```
SELECT COUNT(*) FROM 'Employees' WHERE 'Active'=1;
```

2.	Посчитать количество всех сотрудников на каждой должности.
```
SELECT COUNT(DISTINCT 'JobPosition') FROM 'Employees';
```

3.	Вывести тех сотрудников (FirstName, LastName), у которых заработная плата больше либо равна 2000$.
```
SELECT FirstName,LastName from 'Employees' WHERE 'Salary' >= 2000;
```

4.	Вывести только те должности (JobPosition) сотрудников, где средняя заработная плата превышает 2000$.
```
SELECT JobPosition from (SELECT JobPosition, AVG(salary) as avgsal FROM employees GROUP BY JobPosition) as position 
WHERE avgsal > 2000;
```

5.	Вывести FirstName, LastName только тех сотрудников, имя менеджера которых начинается на букву M.
```
SELECT E.FirstName, E.LastName FROM 'Employees' as empl 
WHERE E.ManagerID = M.ManagerID AND M.FirstName LIKE 'M%';
```

6.	Предположим, что это все записи, которые имеются в таблицах Employees и Managers. 
Какой будет итоговый результат следующего запроса?
```
select e.FirstName, e.LastName, e.ManagerID, m.ManagerID from Employees e
right join Managers m
on e.ManagerID = m.ManagerID
```

| FirstName |	LastName  | ManagerID	| ManagerID |
| --------- | --------- | --------- | --------- |
| John      | Smith     | 147       | 147       |
| Victoria  | Mush      |  147      | 147       |
| Harry     | Thomason  |  258      | 258       |
| null      | null      | null      | 369       |
