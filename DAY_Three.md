# 📅 SQL Day 3 — Table Design & Constraints Review

---

# 🧾 Mini Test

## ✅ Q1: Why do we use PRIMARY KEY?

A **PRIMARY KEY** uniquely identifies each row in a table.

### Key Properties:
- must be unique  
- cannot be NULL  
- only one primary key per table  
- ensures each record is identifiable  

👉 Used as the main identity of a row.

---

## ✅ Q2: Difference Between PRIMARY KEY and UNIQUE

| Feature | PRIMARY KEY | UNIQUE |
|--------|------------|--------|
| Uniqueness | ✅ Required | ✅ Required |
| NULL allowed | ❌ Not allowed | ✅ Allowed (usually one NULL) |
| Count per table | Only one | Multiple allowed |
| Purpose | Row identity | Prevent duplicate values |

👉 **PRIMARY KEY** → main identifier  
👉 **UNIQUE** → prevents duplicates  

---

## ✅ Q3: What happens if NULL is inserted into a NOT NULL column?

The database rejects the operation and throws an error.

Example:
```
ERROR: null value violates not-null constraint
```

👉 NOT NULL ensures mandatory data entry.

---

## ✅ Q4: Why do we use FOREIGN KEY?

A **FOREIGN KEY** creates a relationship between tables and ensures referential integrity.

### It ensures:
- referenced value exists in parent table  
- no invalid references  
- data consistency between tables  

👉 Example: an employee must belong to a valid department.

---

## ✅ Q5: What is DEFAULT used for?

DEFAULT assigns a value automatically when none is provided.

### Example:
```sql
city VARCHAR(50) DEFAULT 'Unknown'
```

If city is not specified during insert → `'Unknown'` is stored.

---

# 🧠 Core Concepts to Remember

✔ PRIMARY KEY → unique row identity  
✔ UNIQUE → prevents duplicate values  
✔ NOT NULL → mandatory field  
✔ FOREIGN KEY → enforces relationships  
✔ DEFAULT → automatic value assignment  

---

# 🛡️ Why Constraints Matter

Without constraints:
- duplicate records
- missing required data
- broken relationships
- unreliable reports

With constraints:
- clean data
- integrity & accuracy
- reliable systems

