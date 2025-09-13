# SQL Injection Labs – PortSwigger Web Security Academy
My notes for portswigger labs on SQLI
> Detailed write-ups and walkthroughs of SQLi labs from PortSwigger Web Security Academy.
 
[here you can find labs](https://portswigger.net/web-security/all-labs#sql-injection)

---

## 📚 Resources

- [SQL Injection Tutorial](https://portswigger.net/web-security/sql-injection)
- [SQL Injection Cheat Sheet](https://portswigger.net/web-security/sql-injection/cheat-sheet)

---

## 🧪 Labs

| No. | Lab Title                                                                                                                          | Category             | Difficulty     | Solution |
|-----|-------------------------------------------------------------------------------------------------------------------------------------|----------------------|----------------|----------|
| 1   | [SQL injection vulnerability in WHERE clause allowing retrieval of hidden data](https://portswigger.net/web-security/sql-injection/lab-retrieve-hidden-data) | In-Band              | 🟢 Apprentice   | [View](sqli-notes.md#lab-sql-injection-vulnerability-in-where-clause-allowing-retrieval-of-hidden-data) |
| 2   | [SQL injection vulnerability allowing login bypass](https://portswigger.net/web-security/sql-injection/lab-login-bypass)           | In-Band (Auth Bypass)| 🟢 Apprentice   | [View](sqli-notes.md#lab-sql-injection-vulnerability-allowing-login-bypass) |
| 3   | [SQL injection with filter bypass via XML encoding](https://portswigger.net/web-security/sql-injection/lab-sql-injection-with-filter-bypass-via-xml-encoding) | In-Band              | 🟡 Practitioner | [View](sqli-notes.md#lab-sql-injection-with-filter-bypass-via-xml-encoding) |
| 4   | [SQL injection attack, querying the database type and version on Oracle](https://portswigger.net/web-security/sql-injection/examining-the-database/lab-querying-database-version-oracle) | Examining DB         | 🟢 Apprentice   | [View](sqli-notes.md#lab-sql-injection-attack-querying-the-database-type-and-version-on-oracle) |
| 5   | [SQL injection attack, querying the database type and version on MySQL and Microsoft](https://portswigger.net/web-security/sql-injection/examining-the-database/lab-querying-database-version-mysql-microsoft) | Examining DB         | 🟢 Apprentice   | [View](sqli-notes.md#lab-sql-injection-attack-querying-the-database-type-and-version-on-mysql-and-microsoft) |
| 6   | [SQL injection attack, listing the database contents on non-Oracle databases](https://portswigger.net/web-security/sql-injection/examining-the-database/lab-listing-database-contents-non-oracle) | Examining DB         | 🟡 Practitioner | [View](sqli-notes.md#lab-sql-injection-attack-listing-the-database-contents-on-non-oracle-databases) |
| 7   | [SQL injection attack, listing the database contents on Oracle](https://portswigger.net/web-security/sql-injection/examining-the-database/lab-listing-database-contents-oracle) | Examining DB         | 🟡 Practitioner | [View](sqli-notes.md#lab-sql-injection-attack-listing-the-database-contents-on-oracle) |
| 8   | [SQL injection UNION attack, determining the number of columns](https://portswigger.net/web-security/sql-injection/union-attacks/lab-determine-number-of-columns) | UNION Attacks        | 🟢 Apprentice   | [View](sqli-notes.md#lab-sql-injection-union-attack-determining-the-number-of-columns-returned-by-the-query) |
| 9   | [SQL injection UNION attack, finding a column containing text](https://portswigger.net/web-security/sql-injection/union-attacks/lab-find-column-containing-text) | UNION Attacks        | 🟢 Apprentice   | [View](sqli-notes.md#lab-sql-injection-union-attack-finding-a-column-containing-text) |
| 10  | [SQL injection UNION attack, retrieving data from other tables](https://portswigger.net/web-security/sql-injection/union-attacks/lab-retrieve-data-from-other-tables) | UNION Attacks        | 🟢 Apprentice   | [View](sqli-notes.md#lab-sql-injection-union-attack-retrieving-data-from-other-tables) |
| 11  | [SQL injection UNION attack, retrieving multiple values in a single column](https://portswigger.net/web-security/sql-injection/union-attacks/lab-retrieve-multiple-values-in-single-column) | UNION Attacks        | 🟡 Practitioner | [View](sqli-notes.md#lab-sql-injection-union-attack-retrieving-multiple-values-in-a-single-column) |
| 12  | [Blind SQL injection with conditional responses](https://portswigger.net/web-security/sql-injection/blind/lab-conditional-responses) | Blind                | 🟡 Practitioner | [View](sqli-notes.md#lab-blind-sql-injection-with-conditional-responses--try-python-script) |
| 13  | [Blind SQL injection with conditional errors](https://portswigger.net/web-security/sql-injection/blind/lab-conditional-errors)     | Blind                | 🟡 Practitioner | [View](sqli-notes.md#lab-blind-sql-injection-with-conditional-errors) |
| 14  | [Visible error-based SQL injection](https://portswigger.net/web-security/sql-injection/blind/lab-sql-injection-visible-error-based) | Blind                | 🟡 Practitioner | [View](sqli-notes.md#lab-visible-error-based-sql-injection) |
| 15  | [Blind SQL injection with time delays](https://portswigger.net/web-security/sql-injection/blind/lab-time-delays)                   | Blind                | 🟢 Apprentice   | [View](sqli-notes.md#lab-blind-sql-injection-with-time-delays) |
| 16  | [Blind SQL injection with time delays and information retrieval](https://portswigger.net/web-security/sql-injection/blind/lab-time-delays-info-retrieval) | Blind                | 🟡 Practitioner | [View](sqli-notes.md#lab-blind-sql-injection-with-time-delays-and-information-retrieval) |
| 17  | [Blind SQL injection with out-of-band interaction](https://portswigger.net/web-security/sql-injection/blind/lab-out-of-band)       | Blind (OOB)          | 🟡 Practitioner | [View](sqli-notes.md#lab-blind-sql-injection-with-out-of-band-interaction) |
| 18  | [Blind SQL injection with out-of-band data exfiltration](https://portswigger.net/web-security/sql-injection/blind/lab-out-of-band-data-exfiltration) | Blind (OOB)          | 🟡 Practitioner | [View](sqli-notes.md#lab-blind-sql-injection-with-out-of-band-data-exfiltration) |
