1) Составьте запрос, который выведет Кастомеров, для которых нет заказов.
```
SELECT * FROM [Customers] WHERE "CustomerID" NOT IN (SELECT "CustomerID" FROM [Orders]);
```
![Screenshot added](https://github.com/NataliiaLebedevaEpam/data-quality-training/blob/main/img/SQL_2_question1.jpg)

2) Посчитать количество продуктов в каждом заказе и вывести максимальное число продуктов среди всех заказов.
```
SELECT MAX(q) FROM (SELECT "OrderID", SUM("Quantity") as q FROM [OrderDetails] GROUP BY "OrderID");
```
![Screenshot added](https://github.com/NataliiaLebedevaEpam/data-quality-training/blob/main/img/SQL_2_question2.jpg)

3) Выбрать самого молодого сотрудника, родившегося в 50-х годах.
```
SELECT * FROM [Employees] WHERE "BirthDate" = (SELECT MAX("BirthDate") FROM [Employees]
 WHERE "BirthDate" BETWEEN '1949-12-31' AND '1960-01-01');
```
![Screenshot added](https://github.com/NataliiaLebedevaEpam/data-quality-training/blob/main/img/SQL_2_question3b.jpg)

4) Посчитать количество кастомеров, которые заказывали продукты, поставляемые из Великобритании и Испании
```
SELECT COUNT() FROM (
SELECT DISTINCT c.CustomerID FROM [Customers] as c 
JOIN [Orders] as o ON c.CustomerID = o.CustomerID 
JOIN [OrderDetails] as od ON o.OrderID = od.OrderID
JOIN [Products] as p ON od.ProductID = p.ProductID
JOIN [Suppliers] as s ON p.SupplierID = s.SupplierID
WHERE s.Country IN ('UK', 'Spain') 
ORDER BY c.CustomerID);
```
![Screenshot added](https://github.com/NataliiaLebedevaEpam/data-quality-training/blob/main/img/SQL_2_question4.jpg)	 

5) Вывести сотрудников таким образом, чтобы сотрудник Dodsworth Anne присутствовал дважды.
Для полученного результата написать запрос, который подсветит наличие дубликатов - выведет дублирующиеся строки - Анну в нашем случае.
```
SELECT * FROM (
	SELECT * FROM [Employees] 
	UNION ALL
	SELECT * FROM [Employees] WHERE "LastName" = 'Dodsworth' AND "FirstName" = 'Anne')
GROUP BY "FirstName", "LastName" HAVING COUNT() > 1;
```
![Screenshot added](https://github.com/NataliiaLebedevaEpam/data-quality-training/blob/main/img/SQL_2_question5.jpg)	 

6) Написать запрос, который сравнит количество символов в колонке Country из Таблицы поставщиков (Suppliers) и кастомеров (Customers).
Добавьте еще одну колонку Result. Если количество символов в колонках совпадает, то тогда значение в колонке Result 'Y', если не совпадает, то 'N'
```
SELECT LENGTH(s.Country) as suppliersCountryLenght, LENGTH(c.Country) as customerCountryLenght,  
CASE WHEN (LENGTH(s.Country) - LENGTH(c.Country)) = 0 THEN 'Y' ELSE 'N' END as isEqualsLenght
FROM [Suppliers] as s, [Customers] as c;
```
![Screenshot added](https://github.com/NataliiaLebedevaEpam/data-quality-training/blob/main/img/SQL_2_question6b.jpg)	 


7) В таблице Suppliers сгруппируйте поставщиков (SupplierName) по первой букве и выведите, какое количество поставщиков приходится на каждую букву. Полученные строки отсортируйте в алфавитном порядке. Результат работы запроса должен иметь приблизительно такой вид:
A - 2 
C - 3 
D -1 
```
SELECT SUBSTR("SupplierName", 1, 1) as startLetter, COUNT(SupplierName) FROM [Suppliers] GROUP BY startLetter ORDER BY startLetter;
```
![Screenshot added](https://github.com/NataliiaLebedevaEpam/data-quality-training/blob/main/img/SQL_2_question7.jpg)	 

8) Вывести кастомеров (customerid, customername), у которых самый высокий по стоимости товар в заказе
```
SELECT DISTINCT c.CustomerID, c.CustomerName FROM Customers as c 
JOIN [Orders] as o ON c.CustomerID = o.CustomerID 
JOIN [OrderDetails] as od ON o.OrderID = od.OrderID
JOIN [Products] as p ON od.ProductID = p.ProductID
GROUP BY c.CustomerID ORDER BY p.Price DESC
LIMIT 1
```
![Screenshot added](https://github.com/NataliiaLebedevaEpam/data-quality-training/blob/main/img/SQL_2_question8c.jpg)


Найдите кастомеров, у которых либо самый высокий товар по стоимости, либо второй по стоимости
```
SELECT DISTINCT c.CustomerID, c.CustomerName FROM Customers as c 
JOIN [Orders] as o ON c.CustomerID = o.CustomerID 
JOIN [OrderDetails] as od ON o.OrderID = od.OrderID
JOIN [Products] as p ON od.ProductID = p.ProductID
GROUP BY c.CustomerID ORDER BY p.Price DESC
LIMIT 1 OFFSET 1;
```
![Screenshot added](https://github.com/NataliiaLebedevaEpam/data-quality-training/blob/main/img/SQL_2_question8b.jpg)	 


Результирующая таблица, содержащая цены на приобретенные товары
![Screenshot added](https://github.com/NataliiaLebedevaEpam/data-quality-training/blob/main/img/SQL_2_question8a.jpg)

