# SQL injection (SQLI)

## key idea

SQLi happens when **user input** is inserted directly into SQL queries without validation → attacker can **change query structure**.

<img title="" src="file:///home/spectre/Desktop/sql-injection.svg" alt="" data-align="inline">

```sql
-- Vulnerable query
SELECT * FROM users WHERE username = '$input' AND password = '$pass';

-- Malicious input
' OR '1'='1
```

---

#### ⚡ Impact

- Unauthorized data access

- Authentication bypass

- Data leakage / modification

- (Severe) Remote code execution

---

## Detection

Test every entry point manually:

- `'` → look for errors/anomalies

- Payloads returning **same vs different values** → compare responses

- Boolean: `OR 1=1` vs `OR 1=2`

- Time delays (`SLEEP`) → check response time

- **OAST (Out-Of-Band Application Security Testing) payloads** → DB tries external connection → if you see it, SQLi confirmed

---

#### Categories

- **In-band SQLi** → easy, direct results
  
  - *Error-based*: use DB error messages
  
  - *Union-based*: use `UNION` to extract data

- **Blind SQLi** → no direct output
  
  - *Boolean-based*: True/False behavior
  
  - *Time-based*: delay injection

- **Out-of-band SQLi** → external channels (DNS/HTTP)

- **Special types**: e.g., second-order SQLi

---

## Blind SQLI

- Occurs when SQL injection exists but:
  
  - HTTP responses do **not** show query results.
  
  - Database error messages are hidden.

- UNION attacks & similar techniques fail since output isn’t visible.

**Key Idea**

- Exploitation relies on **observable differences in application behavior**, not direct query results.

- Example: A “Welcome back” message appears only if query returns data.

**Techniques**

- **Conditional Responses**: Inject conditions and observe different outcomes.
  
  - Example:
    
    - `xyz' AND '1'='1` → true → "Welcome back".
    
    - `xyz' AND '1'='2` → false → no message.

**Data Extraction Process**

- Use conditions to test one piece of data at a time.

- Example: Extracting `Administrator`’s password character by character:
  
  - `SUBSTRING((SELECT Password FROM Users WHERE Username='Administrator'),1,1) > 'm'`
    
    - If true → first character > m.
  
  - `SUBSTRING(...),1,1) > 't'`
    
    - If false → first character ≤ t.
  
  - `SUBSTRING(...),1,1) = 's'`
    
    - True → first character is `s`.

- Repeat for each character → full password retrieved.

**Summary**

- Blind SQLi = no direct query output.

- Exploited by **yes/no conditions** → infer data bit by bit.

---

## 🛡 Prevention

**Golden Rule:** Never build SQL queries by concatenating strings with user input — always bind parameters.

- Use **parameterized queries / prepared statements**

- Use **Web Application Firewalls (WAFs)** as extra layer

- Stored procedures (safe only with parameters)

- **Least privilege** DB accounts

- Validate & sanitize inputs (type, length, format)

---

## 📚 Sources

[SQL Injection Tutorial  | Web Security Academy](https://portswigger.net/web-security/sql-injection)

https://portswigger.net/web-security/sql-injection/cheat-sheet

--- 

'||(SELECT CASE WHEN (1=1) THEN TO_CHAR(1/0) ELSE '' END FROM users where username='administrator')||'

- filters all and return rows have with value `administrator` in username column  and then apply condation on every row from it 

'||(SELECT CASE WHEN (username='administrator) THEN TO_CHAR(1/0) ELSE '' END FROM users )||'
