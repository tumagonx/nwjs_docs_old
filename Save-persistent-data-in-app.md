It's very common to store persistent data in native apps, people usually do it by embedding database libraries or manipulate plain text files. In node-webkit, you have much better choices than that, you can use `Web SQL Database`, `Web Storage` or `Application Cache` without headaches of any extra dependencies.

## Web SQL Database

The `Web SQL Database` API isn't actually part of the HTML5 specification but it is a separate specification which introduces a set of APIs to manipulate client-side databases using SQL. I'll assume you're familiar with basic database operations and SQL language in following guides.

The `Web SQL Database` API in node-webkit is implemented with `sqlite`, and operations are basically the same:

* openDatabase: This method creates the database object either using existing database or creating new one.
* transaction: This method give us the ability to control a transaction and performing either commit or rollback based on the situation.
* executeSql: This method is used to execute actual SQL query.

To create and open a database, use the following code:

```javascript
var db = openDatabase('mydb', '1.0', 'my first database', 2 * 1024 * 1024);
```

I’ve passed four arguments to the openDatabase method. These are:

* Database name
* Version number
* Text description
* Estimated size of database

If you try to open a database that doesn’t exist, the API will create it on the fly for you. You also don’t have to worry about closing databases.

To create table, insert data and query data, just `executeSql` in `transaction`:

```javascript
// Create table and insert one line
db.transaction(function (tx) {
  tx.executeSql('CREATE TABLE IF NOT EXISTS foo (id unique, text)');
  tx.executeSql('INSERT INTO foo (id, text) VALUES (1, "synergies")');
  tx.executeSql('INSERT INTO foo (id, text) VALUES (2, "luyao")');
});

// Query out the data
tx.executeSql('SELECT * FROM foo', [], function (tx, results) {
  var len = results.rows.length, i;
  for (i = 0; i < len; i++) {
    alert(results.rows.item(i).text);
  }
});
```

For more information, you can read tutorials like [Introducing Web SQL Databases](http://html5doctor.com/introducing-web-sql-databases/).

## Web Storage

Web storage is a easy to use key-value database, you can use it like normal js objects but everything will be saved to disk for you.

There are two types of web storage:

* localStorage - stores data with no expiration date
* sessionStorage - stores data for one session

The `localStorage` object stores the data with no expiration date. The data will not be deleted when the browser is closed, and will be available the next day, week, or year.

```javascript
localStorage.love = "luyao";

// Love lasts forever
console.log(localStorage.love);
```

The `sessionStorage` object is equal to the `localStorage` object, except that it stores the data for only one session. The data is deleted when the user closes the window.

```javascript
sessionStorage.life = "";

// But life will have an end
console.log(sessionStorage.life);
```

## Application Cache

HTML5 introduces application cache, which means that a web application is cached, and accessible without an internet connection.

Application cache gives an application three advantages:

* Offline browsing - users can use the application when they're offline
* Speed - cached resources load faster
* Reduced server load - the browser will only download updated/changed resources from the server

However, application cache is designed for browser use, for apps using node-webkit, it's less useful than the other two method, read [HTML5 Application Cache](http://www.w3schools.com/html/html5_app_cache.asp) if you want to use it.