# 📅 SQL Day 1

---

# 🗄️ 1️⃣ Create Table

```sql
CREATE TABLE employees (
  id INT PRIMARY KEY,
  name VARCHAR(50),
  department VARCHAR(50),
  salary INT,
  city VARCHAR(50)
);
```

---

# ➕ 2️⃣ Insert Data

```sql
INSERT INTO employees VALUES
(1, 'Akash', 'IT', 200000, 'Noida'),
(2, 'Gaurav', 'IT', 300000, 'Delhi'),
(3, 'Abhinav', 'IT', 100000, 'Gorakhpur'),
(4, 'Tanisha', 'Sales', 60000, 'Kanpur'),
(5, 'Anisha', 'Sales', 50000, 'Lucknow');
```

---

# 👀 3️⃣ View All Data

```sql
SELECT * FROM employees;
```

---

# 🎯 4️⃣ Show Only Name & Salary

```sql
SELECT name, salary
FROM employees;
```

---

# 📍 5️⃣ Employees from Delhi

```sql
SELECT *
FROM employees
WHERE city = 'Delhi';
```

---

# 🛠️ 6️⃣ Alter Column Type (PostgreSQL)

```sql
ALTER TABLE employees
ALTER COLUMN salary TYPE INTEGER
USING salary::integer;
```

---

# 💰 7️⃣ Salary Greater Than 50,000

```sql
SELECT *
FROM employees
WHERE salary > 50000;
```

---

# 📊 8️⃣ Sort by Salary (Highest First)

```sql
SELECT *
FROM employees
ORDER BY salary DESC;
```

---

# 🌍 9️⃣ Show Unique Cities

```sql
SELECT DISTINCT city
FROM employees;
```

---

# 🥇 🔟 Top 3 Highest Salaries

```sql
SELECT *
FROM employees
ORDER BY salary DESC
LIMIT 3;
```

---

# 🧪 Practice Tasks

## ✅ 1. Employees with salary < 60000

```sql
SELECT *
FROM employees
WHERE salary < 60000;
```

## ✅ 2. Employees from Delhi

```sql
SELECT *
FROM employees
WHERE city = 'Delhi';
```

## ✅ 3. Names in alphabetical order

```sql
SELECT name
FROM employees
ORDER BY name ASC;
```

## ✅ 4. IT employees from Delhi

```sql
SELECT *
FROM employees
WHERE department = 'IT'
AND city = 'Delhi';
```

## ⚠️ 5. Employees earning more than 100000

```sql
SELECT *
FROM employees
WHERE salary > 100000;
```

---

# 🧾 Mini Test Review

## ✅ Q1: Salary between 50,000 and 100,000

```sql
SELECT *
FROM employees
WHERE salary > 50000 AND salary < 100000;
```

### ✅ Alternative (includes boundary values)

```sql
SELECT *
FROM employees
WHERE salary BETWEEN 50000 AND 100000;
```

---

## ✅ Q2: Employees NOT from Delhi

```sql
SELECT *
FROM employees
WHERE city != 'Delhi';
```

### Alternative

```sql
WHERE city <> 'Delhi';
```

---

## ✅ Q3: Departments without duplicates

```sql
SELECT DISTINCT department
FROM employees;
```

---

## ⚠️ Q4: Highest salary

### Your query (sorts highest first but returns all rows)

```sql
SELECT *
FROM employees
ORDER BY salary DESC;
```

### ✅ Highest salary value

```sql
SELECT MAX(salary)
FROM employees;
```

### ✅ Employee with highest salary

```sql
SELECT *
FROM employees
ORDER BY salary DESC
LIMIT 1;
```

---

## ✅ Q5: Names sorted Z → A

```sql
SELECT name
FROM employees
ORDER BY name DESC;
```

---
