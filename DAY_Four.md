# 📅 SQL Day 4 — JOINs (Combining Tables)

JOINs are used to combine data from multiple tables using related columns.

---

# 🗄️ 1️⃣ Create Tables

## Departments Table

```sql
CREATE TABLE departments (
  dep_id INT PRIMARY KEY,
  dep_name VARCHAR(50)
);
```

## Insert Departments

```sql
INSERT INTO departments VALUES
(1, 'IT'),
(2, 'HR'),
(3, 'Finance');
```

---

## Employees Table

```sql
CREATE TABLE employees (
  id INT PRIMARY KEY,
  name VARCHAR(50),
  salary INT,
  dep_id INT,
  FOREIGN KEY (dep_id) REFERENCES departments(dep_id)
);
```

---

## Insert Employees

```sql
INSERT INTO employees VALUES
(1, 'Akash', 50000, 1),
(2, 'Ravi', 40000, 2),
(3, 'Neha', 60000, 1),
(4, 'Simran', 45000, NULL);
```

---

# 🔗 Why JOINs?

Data is stored in separate tables.

| employees | departments |
| --------- | ----------- |
| name      | dep_name    |
| dep_id    | dep_id      |

👉 JOIN connects related data.

Without JOIN → incomplete data  
With JOIN → meaningful results

---

# 🔗 INNER JOIN

Returns only matching rows from both tables.

```sql
SELECT e.name, d.dep_name
FROM employees e
INNER JOIN departments d
ON e.dep_id = d.dep_id;
```

### ✅ Result

Akash → IT  
Ravi → HR  
Neha → IT

❌ Simran excluded (no department)

---

## 🧠 When to use INNER JOIN

✔ when you need matching data only  
✔ most commonly used join  
✔ faster & cleaner results

---

# 🔗 LEFT JOIN

Returns ALL rows from the left table + matching rows from right table.

```sql
SELECT e.name, d.dep_name
FROM employees e
LEFT JOIN departments d
ON e.dep_id = d.dep_id;
```

### ✅ Result

Akash → IT  
Ravi → HR  
Neha → IT  
Simran → NULL

---

## 🧠 When to use LEFT JOIN

✔ to keep all records from main table  
✔ to find missing relationships  
✔ to detect NULL matches

👉 Example: employees without departments

---

# 🔗 RIGHT JOIN

Returns ALL rows from the right table + matching rows from left.

```sql
SELECT e.name, d.dep_name
FROM employees e
RIGHT JOIN departments d
ON e.dep_id = d.dep_id;
```

👉 Rarely used in real projects.

---

# 🧠 INNER vs LEFT vs RIGHT JOIN

| Join Type  | Returns             |
| ---------- | ------------------- |
| INNER JOIN | matching rows only  |
| LEFT JOIN  | all left + matches  |
| RIGHT JOIN | all right + matches |

---

# 🎯 Common Interview Questions

## ❓ Difference between INNER JOIN and LEFT JOIN

**INNER JOIN**

- returns only matching rows

**LEFT JOIN**

- returns all rows from left table
- unmatched rows appear as NULL

---

## ❓ Which JOIN shows unmatched rows?

👉 LEFT JOIN (most common)

---

## ❓ Which JOIN is used most?

👉 INNER JOIN

---

## ❓ Why use table aliases?

```sql
FROM employees e
JOIN departments d
```

✔ shorter queries  
✔ easier to read  
✔ useful with multiple joins

---

## ❓ How to find employees without departments?

```sql
SELECT e.name
FROM employees e
LEFT JOIN departments d
ON e.dep_id = d.dep_id
WHERE d.dep_id IS NULL;
```

⭐ Frequently asked in interviews.

---

## ❓ How to find departments with no employees?

```sql
SELECT d.dep_name
FROM departments d
LEFT JOIN employees e
ON d.dep_id = e.dep_id
WHERE e.id IS NULL;
```

---

# ⭐ Key Takeaways

✔ JOIN combines multiple tables  
✔ INNER JOIN → matching rows  
✔ LEFT JOIN → keep all left rows  
✔ RIGHT JOIN → rarely used  
✔ JOINs reveal meaningful relationships
