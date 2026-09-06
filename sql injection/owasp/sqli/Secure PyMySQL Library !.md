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
```
Attack?
- According to the https://github.com/PyMySQL/PyMySQL/blob/master/pymysql/cursors.py#L156
- The args go to the mogrify( ) then_escape_args()
- So there won't be SQLi
