# Ejercicios Windows Functions

## AVG

- Obtener el promedio de precios por cada categoría de producto. La cláusula OVER(PARTITION BY CategoryID) específica que se debe calcular el promedio de precios por cada valor único de CategoryID en la tabla.

```sql
select c.category_name , p.product_name , p.unit_price ,
avg(p.unit_price) over (partition by c.category_id) as avg_price_bycategory
from products p 
inner join categories c on p.category_id = c.category_id 
```

**Resultado:**

|category_name |product_name                    |unit_price|avg_price_bycategory|
|--------------|--------------------------------|----------|--------------------|
|Beverages     |Guaraná Fantástica              |4,5       |37,9791666667       |
|Beverages     |Ipoh Coffee                     |46        |37,9791666667       |
|Beverages     |Chartreuse verte                |18        |37,9791666667       |
|Beverages     |Côte de Blaye                   |263,5     |37,9791666667       |
|Beverages     |Steeleye Stout                  |18        |37,9791666667       |
|Beverages     |Sasquatch Ale                   |14        |37,9791666667       |
|Beverages     |Lakkalikööri                    |18        |37,9791666667       |
|Beverages     |Rhönbräu Klosterbier            |7,75      |37,9791666667       |
|Beverages     |Outback Lager                   |15        |37,9791666667       |
|Beverages     |Chai                            |18        |37,9791666667       |
|Beverages     |Laughing Lumberjack Lager       |14        |37,9791666667       |
|Beverages     |Chang                           |19        |37,9791666667       |
|Condiments    |Gula Malacca                    |19,45     |22,8541668256       |
|Condiments    |Original Frankfurter grüne Soße |13        |22,8541668256       |
|Condiments    |Northwoods Cranberry Sauce      |40        |22,8541668256       |
|Condiments    |Louisiana Hot Spiced Okra       |17        |22,8541668256       |

- Obtener el promedio de venta de cada cliente

```sql
select c.customer_id , c.contact_name , o.order_id , o.order_date ,
avg(od.quantity * p.unit_price) over (partition by c.customer_id) as avg_order_amount
from customers c 
inner join orders o on c.customer_id = o.customer_id 
inner join order_details od on o.order_id = od.order_id 
inner join products p on od.product_id = p.product_id 
```

**Resultado:**

|customer_id|contact_name      |order_id|order_date|avg_order_amount|
|-----------|------------------|--------|----------|----------------|
|ALFKI      |Maria Anders      |10.702  |1997-10-13|383,0166670481  |
|ALFKI      |Maria Anders      |10.643  |1997-08-25|383,0166670481  |
|ALFKI      |Maria Anders      |10.952  |1998-03-16|383,0166670481  |
|ALFKI      |Maria Anders      |11.011  |1998-04-09|383,0166670481  |
|ALFKI      |Maria Anders      |11.011  |1998-04-09|383,0166670481  |
|ALFKI      |Maria Anders      |10.692  |1997-10-03|383,0166670481  |
|ALFKI      |Maria Anders      |10.643  |1997-08-25|383,0166670481  |
|ALFKI      |Maria Anders      |10.643  |1997-08-25|383,0166670481  |
|ALFKI      |Maria Anders      |10.835  |1998-01-15|383,0166670481  |
|ALFKI      |Maria Anders      |10.952  |1998-03-16|383,0166670481  |
|ALFKI      |Maria Anders      |10.702  |1997-10-13|383,0166670481  |
|ALFKI      |Maria Anders      |10.835  |1998-01-15|383,0166670481  |
|ANATR      |Ana Trujillo      |10.308  |1996-09-18|142,5149991035  |
|ANATR      |Ana Trujillo      |10.926  |1998-03-04|142,5149991035  |
|ANATR      |Ana Trujillo      |10.625  |1997-08-08|142,5149991035  |
|ANATR      |Ana Trujillo      |10.625  |1997-08-08|142,5149991035  |
|ANATR      |Ana Trujillo      |10.625  |1997-08-08|142,5149991035  |
|ANATR      |Ana Trujillo      |10.926  |1998-03-04|142,5149991035  |
|ANATR      |Ana Trujillo      |10.926  |1998-03-04|142,5149991035  |
|ANATR      |Ana Trujillo      |10.926  |1998-03-04|142,5149991035  |
|ANATR      |Ana Trujillo      |10.759  |1997-11-28|142,5149991035  |
|ANATR      |Ana Trujillo      |10.308  |1996-09-18|142,5149991035  |
|ANTON      |Antonio Moreno    |10.365  |1996-11-27|448,0088213752  |
|ANTON      |Antonio Moreno    |10.535  |1997-05-13|448,0088213752  |

- Obtener el promedio de cantidad de productos vendidos por categoría(product_name,quantity_per_unit, unit_price, quantity, avgquantity) y ordenarlo por nombre de la categoría y nombre del producto

```sql
select p.product_name , c.category_name , p.quantity_per_unit ,p.unit_price , od.quantity,
avg(od.quantity) over (partition by c.category_id) as avg_quantity
from products p 
inner join categories c on p.category_id = c.category_id 
inner join order_details od on p.product_id = od.product_id 
order by c.category_name, p.product_name
```

**Resultado:**

|product_name      |category_name|quantity_per_unit |unit_price|quantity|avg_quantity |
|------------------|-------------|------------------|----------|--------|-------------|
|Chai              |Beverages    |10 boxes x 30 bags|18        |10      |23,5940594059|
|Chai              |Beverages    |10 boxes x 30 bags|18        |25      |23,5940594059|
|Chai              |Beverages    |10 boxes x 30 bags|18        |21      |23,5940594059|
|Chai              |Beverages    |10 boxes x 30 bags|18        |60      |23,5940594059|
|Chai              |Beverages    |10 boxes x 30 bags|18        |20      |23,5940594059|
|Chai              |Beverages    |10 boxes x 30 bags|18        |4       |23,5940594059|
|Chai              |Beverages    |10 boxes x 30 bags|18        |10      |23,5940594059|
|Chai              |Beverages    |10 boxes x 30 bags|18        |8       |23,5940594059|
|Chai              |Beverages    |10 boxes x 30 bags|18        |10      |23,5940594059|
|Chai              |Beverages    |10 boxes x 30 bags|18        |40      |23,5940594059|
|Chai              |Beverages    |10 boxes x 30 bags|18        |6       |23,5940594059|
|Chai              |Beverages    |10 boxes x 30 bags|18        |3       |23,5940594059|
|Chai              |Beverages    |10 boxes x 30 bags|18        |15      |23,5940594059|
|Chai              |Beverages    |10 boxes x 30 bags|18        |8       |23,5940594059|
|Chai              |Beverages    |10 boxes x 30 bags|18        |10      |23,5940594059|
|Chai              |Beverages    |10 boxes x 30 bags|18        |18      |23,5940594059|
|Chai              |Beverages    |10 boxes x 30 bags|18        |35      |23,5940594059|
|Chai              |Beverages    |10 boxes x 30 bags|18        |30      |23,5940594059|
|Chai              |Beverages    |10 boxes x 30 bags|18        |15      |23,5940594059|
|Chai              |Beverages    |10 boxes x 30 bags|18        |4       |23,5940594059|
|Chai              |Beverages    |10 boxes x 30 bags|18        |5       |23,5940594059|
|Chai              |Beverages    |10 boxes x 30 bags|18        |20      |23,5940594059|
|Chai              |Beverages    |10 boxes x 30 bags|18        |12      |23,5940594059|
|Chai              |Beverages    |10 boxes x 30 bags|18        |45      |23,5940594059|

---

## MIN

- Selecciona el ID del cliente, la fecha de la orden y la fecha más antigua de la
orden para cada cliente de la tabla 'Orders'.

```sql
select c.customer_id , o.order_date ,
min(o.order_date) over (partition by c.customer_id) as earliest_order_date
from customers c 
inner join orders o on c.customer_id = o.customer_id
```

**Resultado:**

|customer_id|order_date|earliest_order_date|
|-----------|----------|-------------------|
|ALFKI      |1998-01-15|1997-08-25         |
|ALFKI      |1997-10-03|1997-08-25         |
|ALFKI      |1998-04-09|1997-08-25         |
|ALFKI      |1997-10-13|1997-08-25         |
|ALFKI      |1997-08-25|1997-08-25         |
|ALFKI      |1998-03-16|1997-08-25         |
|ANATR      |1997-08-08|1996-09-18         |
|ANATR      |1998-03-04|1996-09-18         |
|ANATR      |1996-09-18|1996-09-18         |
|ANATR      |1997-11-28|1996-09-18         |
|ANTON      |1997-09-22|1996-11-27         |
|ANTON      |1997-05-13|1996-11-27         |
|ANTON      |1998-01-28|1996-11-27         |
|ANTON      |1997-09-25|1996-11-27         |
|ANTON      |1997-04-15|1996-11-27         |
|ANTON      |1997-06-19|1996-11-27         |
|ANTON      |1996-11-27|1996-11-27         |
|AROUT      |1997-02-21|1996-11-15         |
|AROUT      |1997-11-17|1996-11-15         |
|AROUT      |1996-12-16|1996-11-15         |
|AROUT      |1998-03-16|1996-11-15         |
|AROUT      |1997-12-08|1996-11-15         |
|AROUT      |1998-03-03|1996-11-15         |
|AROUT      |1997-12-24|1996-11-15         |
|AROUT      |1997-06-04|1996-11-15         |
|AROUT      |1997-11-14|1996-11-15         |

---

## MAX

- Seleccione el id de producto, el nombre de producto, el precio unitario, el id de
categoría y el precio unitario máximo para cada categoría de la tabla Products.

```sql
select p.product_id , p.product_name , p.unit_price , p.category_id ,
max(p.unit_price) over (partition by p.category_id) as max_unit_price
from products p 
```

**Resultado:**

|product_id|product_name                    |unit_price|category_id|max_unit_price|
|----------|--------------------------------|----------|-----------|--------------|
|24        |Guaraná Fantástica              |4,5       |1          |263,5         |
|43        |Ipoh Coffee                     |46        |1          |263,5         |
|39        |Chartreuse verte                |18        |1          |263,5         |
|38        |Côte de Blaye                   |263,5     |1          |263,5         |
|35        |Steeleye Stout                  |18        |1          |263,5         |
|34        |Sasquatch Ale                   |14        |1          |263,5         |
|76        |Lakkalikööri                    |18        |1          |263,5         |
|75        |Rhönbräu Klosterbier            |7,75      |1          |263,5         |
|70        |Outback Lager                   |15        |1          |263,5         |
|1         |Chai                            |18        |1          |263,5         |
|67        |Laughing Lumberjack Lager       |14        |1          |263,5         |
|2         |Chang                           |19        |1          |263,5         |
|44        |Gula Malacca                    |19,45     |2          |43,9          |
|77        |Original Frankfurter grüne Soße |13        |2          |43,9          |
|8         |Northwoods Cranberry Sauce      |40        |2          |43,9          |
|66        |Louisiana Hot Spiced Okra       |17        |2          |43,9          |
|15        |Genen Shouyu                    |13        |2          |43,9          |
|6         |Grandma's Boysenberry Spread    |25        |2          |43,9          |
|65        |Louisiana Fiery Hot Pepper Sauce|21,05     |2          |43,9          |
|63        |Vegie-spread                    |43,9      |2          |43,9          |

---

## ROW_NUMBER

- Obtener el ranking de los productos más vendidos

```sql
select p.product_name , sum(od.quantity) as total_quantity, row_number() over (order by sum(od.quantity) desc) as rank
from products p 
inner join order_details od on p.product_id = od.product_id 
group by p.product_name 
order by rank
```

**Resultado:**

|product_name                    |total_quantity|rank|
|--------------------------------|--------------|----|
|Camembert Pierrot               |1.577         |1   |
|Raclette Courdavault            |1.496         |2   |
|Gorgonzola Telino               |1.397         |3   |
|Gnocchi di nonna Alice          |1.263         |4   |
|Pavlova                         |1.158         |5   |
|Rhönbräu Klosterbier            |1.155         |6   |
|Guaraná Fantástica              |1.125         |7   |
|Boston Crab Meat                |1.103         |8   |
|Tarte au sucre                  |1.083         |9   |
|Chang                           |1.057         |10  |
|Flotemysost                     |1.057         |11  |
|Sir Rodney's Scones             |1.016         |12  |
|Jack's New England Clam Chowder |981           |13  |
|Lakkalikööri                    |981           |14  |
|Alice Mutton                    |978           |15  |
|Pâté chinois                    |903           |16  |
|Konbu                           |891           |17  |

- Asignar numeros de fila para cada cliente, ordenados por customer_id

```sql
select row_number () over (order by c.customer_id) as row_number, c.customer_id , c.company_name , c.contact_name , c.contact_title
from customers c
group by c.customer_id 
order by row_number
```

**Resultado:**

|row_number|customer_id|company_name|contact_name|contact_title|
|----------|-----------|------------|------------|-------------|
|1|92|Siemens|Engineer||
|2|ALFKI|Alfreds Futterkiste|Maria Anders|Sales Representative|
|3|ANATR|Ana Trujillo Emparedados y helados|Ana Trujillo|Owner|
|4|ANTON|Antonio Moreno Taquería|Antonio Moreno|Owner|
|5|AROUT|Around the Horn|Thomas Hardy|Sales Representative|
|6|BERGS|Berglunds snabbköp|Christina Berglund|Order Administrator|
|7|BLAUS|Blauer See Delikatessen|Hanna Moos|Sales Representative|
|8|BLONP|Blondesddsl père et fils|Frédérique Citeaux|Marketing Manager|
|9|BOLID|Bólido Comidas preparadas|Martín Sommer|Owner|
|10|BONAP|Bon app'|Laurence Lebihan|Owner|
|11|BOTTM|Bottom-Dollar Markets|Elizabeth Lincoln|Accounting Manager|
|12|BSBEV|B's Beverages|Victoria Ashworth|Sales Representative|
|13|CACTU|Cactus Comidas para llevar|Patricio Simpson|Sales Agent|
|14|CENTC|Centro comercial Moctezuma|Francisco Chang|Marketing Manager|
|15|CHOPS|Chop-suey Chinese|Yang Wang|Owner|
|16|COMMI|Comércio Mineiro|Pedro Afonso|Sales Associate|
|17|CONSH|Consolidated Holdings|Elizabeth Brown|Sales Representative|
|18|DRACD|Drachenblut Delikatessen|Sven Ottlieb|Order Administrator|
|19|DUMON|Du monde entier|Janine Labrune|Owner|
|20|EASTC|Eastern Connection|Ann Devon|Sales Agent|
|21|ERNSH|Ernst Handel|Roland Mendel|Sales Manager|
|22|FAMIA|Familia Arquibaldo|Aria Cruz|Marketing Assistant|
|23|FISSA|FISSA Fabrica Inter. Salchichas S.A.|Diego Roel|Accounting Manager|
|24|FOLIG|Folies gourmandes|Martine Rancé|Assistant Sales Agent|

- Obtener el ranking de los empleados más jóvenes () ranking, nombre y apellido del
empleado, fecha de nacimiento)

```sql
select row_number () over (order by min(e.birth_date) desc) as ranking, e.first_name , e.last_name , e.birth_date 
from employees e 
group by e.employee_id 
order by ranking
```

**Resultado:**

|ranking|first_name|last_name|birth_date|
|-------|----------|---------|----------|
|1|Anne|Dodsworth|1966-01-27|
|2|Janet|Leverling|1963-08-30|
|3|Michael|Suyama|1963-07-02|
|4|Robert|King|1960-05-29|
|5|Laura|Callahan|1958-01-09|
|6|Steven|Buchanan|1955-03-04|
|7|Andrew|Fuller|1952-02-19|
|8|Nancy|Davolio|1948-12-08|
|9|Margaret|Peacock|1937-09-19|

---

## SUM

- Obtener la suma de venta de cada cliente

```sql
select sum(od.quantity * od.unit_price) over (order by c.customer_id) as sum_order_amount, o.order_id , c.customer_id , o.employee_id , o.order_date, o.required_date 
from customers c 
inner join orders o on c.customer_id = o.customer_id 
inner join order_details od on o.order_id = od.order_id 
```

**Resultado:**

|sum_order_amount|order_id|customer_id|employee_id|order_date|required_date|
|----------------|--------|-----------|-----------|----------|-------------|
|4596.200004577637|10702|ALFKI|4|1997-10-13|1997-11-24|
|4596.200004577637|10643|ALFKI|6|1997-08-25|1997-09-22|
|4596.200004577637|10952|ALFKI|1|1998-03-16|1998-04-27|
|4596.200004577637|11011|ALFKI|3|1998-04-09|1998-05-07|
|4596.200004577637|11011|ALFKI|3|1998-04-09|1998-05-07|
|4596.200004577637|10692|ALFKI|4|1997-10-03|1997-10-31|
|4596.200004577637|10643|ALFKI|6|1997-08-25|1997-09-22|
|4596.200004577637|10643|ALFKI|6|1997-08-25|1997-09-22|
|4596.200004577637|10835|ALFKI|1|1998-01-15|1998-02-12|
|4596.200004577637|10952|ALFKI|1|1998-03-16|1998-04-27|
|4596.200004577637|10702|ALFKI|4|1997-10-13|1997-11-24|
|4596.200004577637|10835|ALFKI|1|1998-01-15|1998-02-12|
|5999.149994850159|10308|ANATR|7|1996-09-18|1996-10-16|
|5999.149994850159|10926|ANATR|4|1998-03-04|1998-04-01|
|5999.149994850159|10625|ANATR|3|1997-08-08|1997-09-05|

- Obtener la suma total de ventas por categoría de producto

```sql

