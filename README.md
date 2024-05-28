## 1. SQL Query Optimization
Participants are given SQL queries and asked to improve them based on theoretical knowledge of database optimization.

### Provided SQL Queries for Optimization:

  1- **Orders Query:** Retrieve orders with many items and calculate the total price.

```
SELECT Orders.OrderID, SUM(OrderDetails.Quantity * OrderDetails.UnitPrice) AS TotalPrice
FROM Orders
JOIN OrderDetails ON Orders.OrderID = OrderDetails.OrderID
WHERE OrderDetails.Quantity > 10
GROUP BY Orders.OrderID;
```
**Analisis:** En esta consulta considerando que OrderID es una FK y los manejadores de BD generán indices sobre estos, la mejorar que veo es generar un índice **Quantity**

```
CREATE INDEX QuantityIndex ON OrderDetails (Quantity);
```

  2- **Customer Query:** Find all customers from London and sort by CustomerName

```
SELECT CustomerName FROM Customers WHERE City = 'London' ORDER BY CustomerName;
```
**Analisis:** La falta del índice en **City** puede hacer el procesamiento lento

```
CREATE INDEX idx_customer_city ON Customers (City);
```


## 2. NoSQL Query Implementation
Participants will optimize provided NoSQL queries theoretically, focusing on efficient data retrieval and minimized latency.

### NoSQL Queries for Review:

  1- **User Posts Query:** Retrieve the most popular active posts and display their title and like count.

```
db.posts
  .find({ status: "active" }, { title: 1, likes: 1 })
  .sort({ likes: -1 });
```
**Analisis:** Se requiere la creación del siguiente índice para optimizar la consulta

```
db.posts.createIndex({ "status" : 1 , "title" : 1 , likes: -1 });
```

  2- **User Data Aggregation:** Summarize the number of active users by location.

```
db.users.aggregate([
  { $match: { status: "active" } },
  { $group: { _id: "$location", totalUsers: { $sum: 1 } } },
]);
```

En esta operación de agregación hace falta agregar dos índices para optimizar la consulta

```
db.posts.createIndex({ "status" : 1 });
db.posts.createIndex({ "location" : 1 });
```
