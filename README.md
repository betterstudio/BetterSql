# BetterSql Wiki
 
## Get Started

### Java Version 

| Version | Supported          |Recommended       | 
| ------- | ------------------ |------------------|
| > 8.0   | :white_check_mark: |:x:               |
| 8.0.x   | :white_check_mark: |:white_check_mark:|
| < 8.0   | :x:                |:x:               |

### Maven

```xml
<repositories>
  <repository>
    <id>jitpack.io</id>
    <url>https://jitpack.io</url>
  </repository>
</repositories>

<dependencies>
  <dependency>
    <groupId>com.github.betterstudio</groupId>
    <artifactId>BetterSql</artifactId>
    <version>1.7</version>
  </dependency>
</dependencies>
```
### Gradle

```gradle
allprojects {
  repositories {
    maven { url 'https://jitpack.io' }
  }
}

dependencies {
  implementation 'com.github.betterstudio:BetterSql:1.7'
}
```
[![](https://jitpack.io/v/betterstudio/BetterSql.svg)](https://jitpack.io/#betterstudio/BetterSql)

# How To Use

## Create a Database 
To start, we will have to create Database. 
Just make a Database object with SqliteDatabase or HikariDatabase, and use the function ``Database#connect();``
```Java
 Database db = new Database("C:/Users/KEVIN/Desktop", "storage");
```

## Create Your Table
To create your table, simply create a table object with the function ``Database#getTable(String name, List <TableArguments> args); ``. The `` TableArguments`` object is composed of a type, a name, a maximum value, and finally a default type. If you want to know the usefulness of each of the enumerations, these are specified in the .java. After that, just make a `` Table#createTable(); ``. If the default type is DEFAULT_VALUE, you must add ``TableArguments#setData(String data);``
```Java
List<TableArguments> tbArgs = new ArrayList<>();
tbArgs.add(new TableArguments(TableArgumentsType.VARCHAR, "name", 32, TableDefaultArgumentsType.NO));
tbArgs.add(new TableArguments(TableArgumentsType.INT, "age", TableDefaultArgumentsType.NO));
tbArgs.add(new TableArguments(TableArgumentsType.TEXT, "bio", 1024, TableDefaultArgumentsType.DEFAULT_VALUE).setData("Default Biographie Set !"));

Table t = db.getTable("city", tbArgs);
t.createTable();
```
Use Help class, it's more swift ! It allows you to make shortcuts like the function ``H#args(String type, String name, int value, String default);`` Don't use the enumerations, but only the first 2 letters, for more explanation, just look directly in the .java of the enum. In 0.0.6, you have new function, ``H.varArgs(String name, int value);`` (They are : ``varArgs``, ``intArgs`` and ``textArgs``. They return an ArgumentsType like the function name and a NO_NULL default value.

```Java
//That was the same result

Table t = db.getTable("city", Help.args("va", "name", 32), Help.args("in", "age", 1024, "no"), Help.args("te", "bio", 1024, "no");
Table t = db.getTable("city", Help.varArgs("name", 32), Help.intArgs("age", 4), Help.textArgs("bio", 1024));
t.createTable();
```
## Request

### Add Request

To insert a line in a database you just have to use the function ``Table#add(Object...)``, don't forget the ``Request#sendSql();``.
```java
t.add("Kevin", 14, "A very good baker!").sendSql();
```

### Update Request

To update a database, use the function ``Table#updateAll(Object...);``, and ``Request#sendSql()`` afterwards. To make it faster, you can always use my Help class which has a function to create HashMap, ``H#hash(List<String>, List<Object>);`` You can add the function ``Request#where(String, Object);`` which adds a condition, this condition is mandatory for certain request like select or remove.
```java
t.update("Kevin", 15, "My new Biographie").where("name", "KEVIN").sendSql();
```

### Select Request

To Select a row in a database, you must use the ``Table#select()`` function, with a ``Request#where(String, Object)`` condition, with a ``SelectRequest#sendSql();``.  Finally use ``SqlResult#get()``. If the list is empty, the condition return false, and therefore the line you are looking for does not exist.``. If you want get all the rows, you must not use any condition and replace the ``get()`` with a ``gets()``.
```java
String bio;
int age;
try{
   List<Object> all = t.select().where("name", "Kev'").sendSql().get();
   //If the player was in the database
   age = (int) all.get(1);
   bio = (String) all.get(2);
}catch(BetterSqlException exception){ 
   //Do when player isn't in the database
   //Exemple :
   t.add("Kevin", 14, "A default Biographie").sendSql();
   age = (int) 14;
   bio = (String) "A default Biographie"; 
}
```
### Delete Request 

To delete a Row in a database, you must use ``Table#delete()`` function and a condition. Don't forget to use ``RemoveRequest#sendSql()`` 
to send your request.

```java
t.delete().where("name", "Kevin").sendSql();
```

## Thanks !

Thanks for using my library, if you want to give me a donations don't wait x) 
