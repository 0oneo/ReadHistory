SQL 阅读记录
======

### Select

`SELECT DISTINCT` statement use the combination of the columns selected from table as the factor to decide distinct.

| name | email |
| ---- | ----- |
| foo | haha@gmail.com |
| foo | hehe@gmail.com |
| bar | hehe@gmail.com |

```SQL
SELECT DISTINCT name, email FROM table_name;
```

will get the following results:

| name | email |
| ---- | ----- |
| foo | haha@gmail.com |
| foo | hehe@gmail.com |
| bar | hehe@gmail.com |


----

### SQL injection

SQL injection is a technique where malicious users can inject SQL commands into an SQL statement, via web page input.

#### Based on `1=1` always true

```SQL
SELECT * FROM Users WHERE UserId = 105 or 1=1
```

#### Based on `"'='"` always true
Server
```ruby
uName = getRequestString("UserName");
uPass = getRequestString("UserPass");

sql = 'SELECT * FROM Users WHERE Name ="' + uName + '" AND Pass ="' + uPass + '"'
```

#### Based on Batched SQL statements

```ruby
txtUserId = getRequestString("UserId");
txtSQL = "SELECT * FROM Users WHERE UserId = " + txtUserId;
```

and `txtUserId` is `105; DROP TABLE Suppliers`

#### Protection

The only proven way to protect a web site from SQL injection attacks, is to use SQL parameters.

```ruby
txtUserId = getRequestString("UserId");
txtSQL = "SELECT * FROM Users WHERE UserId = @0";
db.Execute(txtSQL,txtUserId);
```

----

### How SQL join works
Here is an article trying to explain how table join works visually, [link](http://www.codeproject.com/Articles/33052/Visual-Representation-of-SQL-Joins), I also get pdf version in current directory.

As see the table as set, but what's in the set, the components is specified by the where cause. Here is a demo:

```SQL
SELECT user.name, user.email, book.isbn, book.name FROM user INNER JOIN book WHERE user.id == book.owner_id
```

Above SQL statements see `user` and `book` tables as sets of user ids.
