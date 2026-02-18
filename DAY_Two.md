# 📅 SQL Day 2 — INSERT, UPDATE & DELETE (CRUD)

CRUD = Create, Read, Update, Delete

---

# ➕ 1️⃣ Insert a New Employee

```sql
INSERT INTO employees (id, name, department, salary, city)
VALUES (7, 'Mohit', 'Finance', 40000, 'Ghaziabad');
```

---

# ➕ 2️⃣ Insert With Partial Columns

```sql
INSERT INTO employees (id, name)
VALUES (8, 'Abhinav2');
```

👉 `department`, `salary`, and `city` will become **NULL**

---

# ✏️ 3️⃣ Update Salary of Abhinav2

```sql
UPDATE employees
SET salary = 50000
WHERE name = 'Abhinav2';
```

---

# ✏️ 4️⃣ Update Multiple Columns

```sql
UPDATE employees
SET salary = 10000,
    city = 'Moradabad'
WHERE name = 'Abhinav2';
```

---

# ⚠️ IMPORTANT WARNING

❌ NEVER run UPDATE without WHERE as This updates EVERY row.

```sql
UPDATE employees
SET salary = 0;
```

---

# ❌ 5️⃣ Delete a Specific Employee

```sql
DELETE FROM employees
WHERE id = 8;
```

✔ Safe deletion

---

# ⚠️ 6️⃣ Delete ALL Records

```sql
DELETE FROM employees;
```

🚨 Removes all rows but keeps table structure.

---

# ❓ 7️⃣ Find Rows With NULL Salary

```sql
SELECT *
FROM employees
WHERE salary IS NULL;
```

👉 `NULL` means missing or unknown value.

---

# ✏️ 8️⃣ Update NULL Values

```sql
UPDATE employees
SET salary = 30000
WHERE salary IS NULL;
```

---

# ✏️ 9️⃣ Set City = 'Unknown' Where City is NULL

❌ Incorrect:

```sql
WHERE city = NULL;
```

✅ Correct:

```sql
UPDATE employees
SET city = 'Unknown'
WHERE city IS NULL;
```

👉 NULL must be checked using `IS NULL`

---

# ➕ 🔟 Insert a Record with NULL Salary

## Option 1: Omit salary column

```sql
INSERT INTO employees (id, name, department, city)
VALUES (9, 'Rahul', 'Sales', 'Delhi');
```

## Option 2: Explicit NULL

```sql
INSERT INTO employees
VALUES (9, 'Rahul', 'Sales', NULL, 'Delhi');
```

---

# ⭐ Key Learnings

- DELETE removes data
- NULL represents missing values
- Use `IS NULL` instead of `= NULL`
- Always use WHERE in UPDATE & DELETE
- Inserting duplicate primary keys
