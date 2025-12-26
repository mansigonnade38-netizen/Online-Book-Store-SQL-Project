# 📚 Online-Book-Store-SQL-Project

## 🔍 Project Overview
This project demonstrates end-to-end online bookstore database management using PostgreSQL.
It includes database creation, table setup, data import, and analytical SQL queries to explore books, customers, and order data.

This project is designed as a Data Analyst.

## 🧾 Dataset Description
- Files: Books.csv, Customers.csv, Orders.csv

- Data Type: Transaction-level bookstore data

- Attributes:

  📘 Books Table:

- Book_ID

- Title

- Author

- Genre

- Published_Year

- Price

- Stock

👥 Customers Table:

- Customer_ID

- Name

- Email

- Phone

- City

- Country

🛒 Orders Table:

- Order_ID

- Customer_ID

- Book_ID

- Order_Date

- Quantity

- Total_Amount

#🛠️ Tools & Technologies

- Database: PostgreSQL

- Tool: pgAdmin

- Language: SQL

## 🏗️ Database & Table Creation
```sql
-- Create Database
CREATE DATABASE OnlineBookstore;

-- Switch to the database
\c OnlineBookstore;

-- Books Table
DROP TABLE IF EXISTS Books;
CREATE TABLE Books (
    Book_ID SERIAL PRIMARY KEY,
    Title VARCHAR(100),
    Author VARCHAR(100),
    Genre VARCHAR(50),
    Published_Year INT,
    Price NUMERIC(10,2),
    Stock INT
);

-- Customers Table
DROP TABLE IF EXISTS Customers;
CREATE TABLE Customers (
    Customer_ID SERIAL PRIMARY KEY,
    Name VARCHAR(100),
    Email VARCHAR(100),
    Phone VARCHAR(15),
    City VARCHAR(50),
    Country VARCHAR(150)
);

-- Orders Table
DROP TABLE IF EXISTS Orders;
CREATE TABLE Orders (
    Order_ID SERIAL PRIMARY KEY,
    Customer_ID INT REFERENCES Customers(Customer_ID),
    Book_ID INT REFERENCES Books(Book_ID),
    Order_Date DATE,
    Quantity INT,
    Total_Amount NUMERIC(10,2)
);
```

## 🔄 Data Import
```sql
-- Import data from CSV files
COPY Books(Book_ID, Title, Author, Genre, Published_Year, Price, Stock)
FROM 'Books.csv' CSV HEADER;

COPY Customers(Customer_ID, Name, Email, Phone, City, Country)
FROM 'Customers.csv' CSV HEADER;

COPY Orders(Order_ID, Customer_ID, Book_ID, Order_Date, Quantity, Total_Amount)
FROM 'Orders.csv' CSV HEADER;
```

# 📊 SQL Queries & Answers

## 🔹 Basic Queries

### 1️⃣ Retrieve all books in the "Fiction" genre
```sql
SELECT * FROM Books WHERE Genre = 'Fiction';
```

### 2️⃣ Find books published after 1950
```sql
SELECT * FROM Books WHERE Published_Year > 1950;
```


### 3️⃣ List all customers from Canada
```sql
SELECT * FROM Customers WHERE Country = 'Canada';
```


### 4️⃣ Show orders placed in November 2023
```sql
SELECT * FROM Orders
WHERE Order_Date BETWEEN '2023-11-01' AND '2023-11-30';
```


### 5️⃣ Retrieve total stock of all books
```sql
SELECT SUM(Stock) AS Total_Stock FROM Books;
```


### 6️⃣ Find the most expensive book
```sql
SELECT * FROM Books ORDER BY Price DESC LIMIT 1;
```


### 7️⃣ Show orders where quantity > 1
```sql
SELECT * FROM Orders WHERE Quantity > 1;
```


### 8️⃣ Retrieve orders with total amount greater than $20
```sql
SELECT * FROM Orders WHERE Total_Amount > 20;
```


### 9️⃣ List all available book genres
```sql
SELECT DISTINCT Genre FROM Books;
```


### 🔟 Find the book with the lowest stock
```sql
SELECT * FROM Books ORDER BY Stock ASC LIMIT 1;
```


### 1️⃣1️⃣ Calculate total revenue from all orders
```sql
SELECT SUM(Total_Amount) AS Total_Revenue FROM Orders;
```


## 🚀 Advanced Queries

### 1️⃣ Total number of books sold for each genre
```sql
SELECT b.Genre, SUM(o.Quantity) AS Total_Books_Sold
FROM Orders o
JOIN Books b ON o.Book_ID = b.Book_ID
GROUP BY b.Genre;
```


### 2️⃣ Average price of books in the Fantasy genre
```sql
SELECT AVG(Price) AS Average_Price
FROM Books
WHERE Genre = 'Fantasy';
```


### 3️⃣ Customers who placed at least 2 orders
```sql
SELECT c.Name, COUNT(o.Order_ID) AS Order_Count
FROM Orders o
JOIN Customers c ON o.Customer_ID = c.Customer_ID
GROUP BY c.Name
HAVING COUNT(o.Order_ID) >= 2;
```


### 4️⃣ Most frequently ordered book
```sql
SELECT b.Title, COUNT(o.Order_ID) AS Order_Count
FROM Orders o
JOIN Books b ON o.Book_ID = b.Book_ID
GROUP BY b.Title
ORDER BY Order_Count DESC
LIMIT 1;
```


### 5️⃣ Top 3 most expensive Fantasy books
```sql
SELECT * FROM Books
WHERE Genre = 'Fantasy'
ORDER BY Price DESC
LIMIT 3;
```

### 6️⃣ Total quantity of books sold by each author
```sql
SELECT b.Author, SUM(o.Quantity) AS Total_Books_Sold
FROM Orders o
JOIN Books b ON o.Book_ID = b.Book_ID
GROUP BY b.Author;
```

### 7️⃣ Cities where customers spent over $30
```sql
SELECT DISTINCT c.City
FROM Orders o
JOIN Customers c ON o.Customer_ID = c.Customer_ID
WHERE o.Total_Amount > 30;
```

### 8️⃣ Customer who spent the most
```sql
SELECT c.Name, SUM(o.Total_Amount) AS Total_Spent
FROM Orders o
JOIN Customers c ON o.Customer_ID = c.Customer_ID
GROUP BY c.Name
ORDER BY Total_Spent DESC
LIMIT 1;
```


### 9️⃣ Remaining stock after fulfilling orders
```sql
SELECT b.Title,
       b.Stock - COALESCE(SUM(o.Quantity), 0) AS Remaining_Stock
FROM Books b
LEFT JOIN Orders o ON b.Book_ID = o.Book_ID
GROUP BY b.Title, b.Stock;
```

## ✅ Conclusion
This Online Bookstore SQL Project successfully demonstrates how structured data can be transformed into meaningful business insights using PostgreSQL.

Through the analysis:

- We identified total books sold for each genre, helping understand customer preferences and high-demand categories

- Analyzed top-selling and most frequently ordered books

- Evaluated customer purchasing behavior and high-value customers

- Calculated total revenue and remaining stock after sales, which is crucial for inventory management

- Used JOINs, aggregations, filtering, and grouping to solve real-world business questions

