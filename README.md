# 🎓 University Database — SQL Schema & Seed Data

A relational database schema and seed data for a **University Database**, based on the schema from *Database System Concepts, 7th Edition* by Silberschatz, Korth, and Sudarshan (Figure 2.9).

---

## 📂 Repository Structure

| File | Description |
|------|-------------|
| [`DDL.sql`](DDL.sql) | **Data Definition Language** — Creates all 10 tables with primary keys, foreign keys, and CHECK constraints. |
| [`SEED_data.sql`](SEED_data.sql) | **Seed Data** — Populates every table with sample university data (classrooms, departments, courses, instructors, students, etc.). |
| [`Problem Set 1.pdf`](Problem%20Set%201.pdf) | Practice problems / assignments to run against the database. |

---

## 🗃️ Schema Overview

The database models a university system with the following **10 tables**:

```
classroom ──┐
             ├──► section ──► teaches ──► instructor
course ──────┘        │                       │
                      ▼                       ▼
                    takes ──► student ──► advisor
                                │
department ◄────────────────────┘
time_slot
prereq ──► course
```

| Table | Description |
|-------|-------------|
| `classroom` | Buildings and room numbers with seating capacity |
| `department` | Academic departments with building and budget |
| `course` | Courses offered, linked to departments |
| `instructor` | Faculty members with salary and department |
| `section` | Specific course sections (semester, year, room) |
| `teaches` | Which instructor teaches which section |
| `student` | Students with total credits and department |
| `takes` | Which student takes which section (with grade) |
| `advisor` | Student–instructor advising relationships |
| `time_slot` | Weekly time slots (day, start/end times) |
| `prereq` | Course prerequisite relationships |

---

## ⚠️ Important: Execution Order

> [!IMPORTANT]
> Always run **`DDL.sql` first**, then **`SEED_data.sql`**. The seed data depends on the tables and foreign key relationships created by the DDL script.

---

## 🚀 How to Run

### 1. MariaDB

#### Prerequisites
- [MariaDB Server](https://mariadb.org/download/) installed
- Access to the `mariadb` (or `mysql`) CLI client

#### Steps

```bash
# Step 1: Log in to MariaDB
mariadb -u root -p

# Step 2: Create a new database
CREATE DATABASE university;
USE university;

# Step 3: Exit the MariaDB shell, then run the SQL files from terminal
mariadb -u root -p university < DDL.sql
mariadb -u root -p university < SEED_data.sql

# Step 4: Verify
mariadb -u root -p university -e "SHOW TABLES;"
mariadb -u root -p university -e "SELECT * FROM student;"
```

**Alternative — run from inside the MariaDB shell:**

```sql
CREATE DATABASE university;
USE university;
SOURCE /full/path/to/DDL.sql;
SOURCE /full/path/to/SEED_data.sql;
```

> [!NOTE]
> MariaDB is fully compatible with the SQL syntax used in these files. No modifications are needed.

---

### 2. MySQL & MySQL Workbench

#### Prerequisites
- [MySQL Server](https://dev.mysql.com/downloads/mysql/) installed
- [MySQL Workbench](https://dev.mysql.com/downloads/workbench/) (optional GUI)

#### Option A: MySQL Command Line

```bash
# Step 1: Log in
mysql -u root -p

# Step 2: Create and use the database
CREATE DATABASE university;
USE university;
EXIT;

# Step 3: Import the files
mysql -u root -p university < DDL.sql
mysql -u root -p university < SEED_data.sql

# Step 4: Verify
mysql -u root -p university -e "SELECT * FROM department;"
```

#### Option B: MySQL Workbench (GUI)

1. Open **MySQL Workbench** and connect to your MySQL server instance.
2. Click the **Create Schema** icon (or run `CREATE DATABASE university;`) and name it `university`.
3. Double-click the `university` schema in the left sidebar to set it as the default.
4. Go to **File → Open SQL Script…** and open `DDL.sql`.
5. Click the ⚡ **Execute** button (or press `Ctrl+Shift+Enter`) to run the script.
6. Open `SEED_data.sql` the same way and execute it.
7. Verify by running:
   ```sql
   SELECT * FROM student;
   SELECT * FROM course;
   ```

> [!TIP]
> In MySQL Workbench, make sure the correct schema is selected (bold in the sidebar) before executing scripts. You can also right-click the schema → **Set as Default Schema**.

---

### 3. XAMPP (MySQL via phpMyAdmin)

#### Prerequisites
- [XAMPP](https://www.apachefriends.org/download.html) installed

#### Steps

1. **Start XAMPP** and ensure **Apache** and **MySQL** modules are running.
2. Open your browser and go to **[http://localhost/phpmyadmin](http://localhost/phpmyadmin)**.
3. Click the **"New"** link in the left sidebar to create a new database.
4. Enter `university` as the database name, select **`utf8_general_ci`** as the collation, and click **Create**.
5. Select the `university` database from the left sidebar.
6. Go to the **"Import"** tab at the top.
7. Click **"Choose File"**, select **`DDL.sql`**, and click **"Go"** to execute.
8. Repeat the import for **`SEED_data.sql`**.
9. Click the **"SQL"** tab to run queries, or browse individual tables from the left sidebar.

#### Alternative — XAMPP MySQL Shell

```bash
# Navigate to the XAMPP MySQL binary directory

# On Linux:
/opt/lampp/bin/mysql -u root

# On Windows:
C:\xampp\mysql\bin\mysql.exe -u root

# Then run:
CREATE DATABASE university;
USE university;
SOURCE /full/path/to/DDL.sql;
SOURCE /full/path/to/SEED_data.sql;
```

> [!NOTE]
> XAMPP bundles **MariaDB** (not Oracle MySQL) in recent versions, so full compatibility is guaranteed with these SQL files.

---

### 4. PostgreSQL

#### Prerequisites
- [PostgreSQL](https://www.postgresql.org/download/) installed
- Access to `psql` CLI or [pgAdmin](https://www.pgadmin.org/) GUI

#### Steps (psql CLI)

```bash
# Step 1: Log in to PostgreSQL
psql -U postgres

# Step 2: Create the database and connect to it
CREATE DATABASE university;
\c university

# Step 3: Exit psql, then import the SQL files
psql -U postgres -d university -f DDL.sql
psql -U postgres -d university -f SEED_data.sql

# Step 4: Verify
psql -U postgres -d university -c "SELECT * FROM student;"
```

**Alternative — run from inside psql:**

```sql
CREATE DATABASE university;
\c university
\i /full/path/to/DDL.sql
\i /full/path/to/SEED_data.sql
```

#### Steps (pgAdmin GUI)

1. Open **pgAdmin** and connect to your PostgreSQL server.
2. Right-click **Databases → Create → Database…** and name it `university`.
3. Right-click the `university` database → **Query Tool**.
4. Click the 📂 **Open File** icon, select `DDL.sql`, and click ▶ **Execute/Run** (or press `F5`).
5. Open `SEED_data.sql` the same way and execute it.
6. Verify by running:
   ```sql
   SELECT * FROM student;
   SELECT * FROM course;
   ```

> [!NOTE]
> The SQL syntax in these files (`VARCHAR`, `NUMERIC`, `CHECK` constraints, `ON DELETE` actions) is **fully compatible with PostgreSQL**. No modifications are needed.

---

## 🔍 Sample Queries

Once the database is set up, try these queries to explore the data:

```sql
-- List all Computer Science courses
SELECT * FROM course WHERE dept_name = 'Comp. Sci.';

-- Find all instructors earning more than $80,000
SELECT name, dept_name, salary FROM instructor WHERE salary > 80000;

-- Show students and the courses they've taken with grades
SELECT s.name AS student, c.title AS course, t.grade
FROM student s
JOIN takes t ON s.ID = t.ID
JOIN course c ON t.course_id = c.course_id
ORDER BY s.name;

-- List departments and their total number of instructors
SELECT dept_name, COUNT(*) AS num_instructors
FROM instructor
GROUP BY dept_name
ORDER BY num_instructors DESC;

-- Find prerequisites for each course
SELECT c.title AS course, p.title AS prerequisite
FROM prereq pr
JOIN course c ON pr.course_id = c.course_id
JOIN course p ON pr.prereq_id = p.course_id;
```

---

## 📋 Compatibility Notes

| Feature | MariaDB | MySQL | XAMPP (MariaDB) | PostgreSQL |
|---------|:-------:|:-----:|:---------------:|:----------:|
| `VARCHAR` | ✅ | ✅ | ✅ | ✅ |
| `NUMERIC(p, s)` | ✅ | ✅ | ✅ | ✅ |
| `CHECK` constraints | ✅ (10.2+) | ✅ (8.0.16+) | ✅ | ✅ |
| `ON DELETE CASCADE` | ✅ | ✅ | ✅ | ✅ |
| `ON DELETE SET NULL` | ✅ | ✅ | ✅ | ✅ |
| `INSERT INTO ... VALUES` | ✅ | ✅ | ✅ | ✅ |

> [!TIP]
> If you are using **MySQL < 8.0.16**, `CHECK` constraints will be parsed but **silently ignored**. The tables will still be created and data will load, but constraints like `CHECK (salary > 29000)` won't be enforced. Consider upgrading to MySQL 8.0.16+ or MariaDB 10.2+ for full CHECK constraint support.

---

## 📚 Reference

- **Textbook:** *Database System Concepts*, 7th Edition — Silberschatz, Korth, Sudarshan
- **Schema:** Figure 2.9 — University Database Schema Diagram
- **Sample Data:** Appendix A — Small Relations

---

## 📄 License

This project uses the sample schema and data from *Database System Concepts* for **educational purposes only**.

