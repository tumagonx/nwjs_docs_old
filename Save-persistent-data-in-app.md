It's very common to store persistent data in native apps, people usually do it by embedding external databases or manipulating plain text files. In node-webkit, you have much better choices than that, you can use `Web SQL Database`, `embedded databases`, `Web Storage` or `Application Cache` without headaches of any extra dependencies.

## Saving a plain text file directly to disk

NW.js provides [`App.dataPath`](https://github.com/nwjs/nw.js/wiki/App) which will give you a system dependent path where you can store application data. For example, on Windows this would typically be `C:\Users\username\AppData\Local\YourAppName`, on Linux it would be here `/home/username/.config/YourAppName`.

**Real World Example:**

```js
var nw = require('nw.gui'); // This line is only required for NW.js 0.12.x and below
var fs = require('fs');
var path = require('path');

var helpers = {
  file: 'my-settings-file.json',
  filePath: path.join(nw.App.dataPath, this.file),
  saveSettings: function (settings, callback) {
    fs.writeFile(this.filePath, JSON.stringify(settings), function (err) {
      if (err) {
        console.info('There was an error attempting to save your settings.');
        console.warn(err.message);
        return;
      } else if (typeof(callback) === 'function') {
        callback();
      }
    });
  },
  loadSettings: function (callback) {
    fs.readFile(this.filePath, function (err, data) {
      if (err) {
        console.info('There was an error attempting to read your settings.');
        console.warn(err.message);
      } else if (typeof(callback) === 'function') {
        try {
          data = JSON.parse(data);
          callback(data);
        } catch (error) {
          console.info('There was a problem parsing the data in your settings.');
          console.warn(error);
        }
      }
    });
  }
};

var mySettings = {
  language: 'en',
  theme: 'dark'
};

helpers.saveSettings(mySettings, function () {
  console.log('Settings saved');
});

helpers.loadSettings(function (data) {
  mySettings = data;
});
```

## Web SQL Database

The `Web SQL Database` API isn't actually part of the HTML5 specification but it is a separate specification which introduces a set of APIs to manipulate client-side databases using SQL. I'll assume you're familiar with basic database operations and SQL language in following guides.

The `Web SQL Database` API in node-webkit is implemented with `sqlite`, and operations are basically the same:

* openDatabase: This method creates the database object either using existing database or creating new one.
* transaction: This method give us the ability to control a transaction and performing either commit or rollback based on the situation.
* executeSql: This method is used to execute actual SQL query.

To create and open a database, use the following code:

```javascript
var databaseName = 'mydb';
var versionNumber = '1.0';
var textDescription = 'my first database';
var estimatedSizeOfDatabase = 2 * 1024 * 1024;

var db = openDatabase(
    databaseName,
    versionNumber,
    textDescription,
    estimatedSizeOfDatabase
);
```

If you try to open a database that doesn't exist, the API will create it on the fly for you. You also don't have to worry about closing databases.

To create a table, insert data or query data, use `executeSql` in `transaction`:

```javascript
// Create table and insert one line
db.transaction(function (tx) {
  tx.executeSql('CREATE TABLE IF NOT EXISTS foo (id unique, text)');
  tx.executeSql('INSERT INTO foo (id, text) VALUES (1, "synergies")');
  tx.executeSql('INSERT INTO foo (id, text) VALUES (2, "luyao")');
});

// Query out the data
db.transaction(function (tx) {
  tx.executeSql('SELECT * FROM foo', [], function (tx, results) {
    var len = results.rows.length, i;
    for (i = 0; i < len; i++) {
      alert(results.rows.item(i).text);
    }
  });
});
```

For more information, you can read tutorials like [Introducing Web SQL Databases](http://html5doctor.com/introducing-web-sql-databases/).


## IndexedDB
[IndexedDB](https://developer.mozilla.org/en-US/docs/IndexedDB) is considered the NoSQL successor of WebSQL. It is supported by the latest versions of Chromium and therefore by node-webkit - the implementation is based on the key-value storage LevelDB.
IndexedDB's API is asynchronous and relatively low-level and verbose, so you might prefer using an abstraction, like PouchDB.

## PouchDB
[PouchDB](http://pouchdb.com/) is an embedded database engine inspired by CouchDB. In NW.js, it can be used as an abstraction over IndexedDB or WebSQL (via the Chromium implementations) or directly on LevelDB (via the node.js module).

As with CouchDB, there are no dynamic index-based queries, but you can dynamically aggregate a view via map/reduce. It can directly replicate to/from CouchDB, which gives it an advantage if you are building an application that has to sync with the cloud.

To get started with PouchDB and NW.js, check out [pouchdb-nw](https://github.com/nolanlawson/pouchdb-nw).

Additional links you may find useful:

* https://github.com/pouchdb-community/relational-pouch
* https://www.npmjs.com/package/pouchdb-find

## EJDB
[EJDB](https://github.com/Softmotions/ejdb) (Embedded JSON Database engine) is a simple & fast database engine based on Tokyo Cabinet. It's usage copies MongoDB - you can easily make dynamic queries and sort/paginate the result.
The fast queries and easy-to-use API make it a very good choice for node-webkit.
NB: An extra step is required to use EJDB in a node-webkit app. Please refer to [3rd party modules with C/C++ addons](https://github.com/rogerwang/node-webkit/wiki/Using-Node-modules) for information.

## NeDB
[NeDB](https://github.com/louischatriot/nedb) (Node embedded database) is a pure javascript database for Node.js (unlike EJDB, you don't need to compile anything). It implements the most common subset of MongoDB's and can be used to persist data or simply as an in-memory datastore. Even though it's not native, it's still fast enough for desktop apps (40k reads/s, 10k writes/s).

## LinvoDB
[LinvoDB](https://github.com/Ivshti/linvodb3) is a persistent embedded database for Node.js / NW.js. It can be used on top of LevelDB, but it can also be used on top of Medea with no need to compile anything. It has a MongoDB+Mongoose-like features and API. Performance is comparible to MongoDB.


## MarsDB
[MarsDB](https://github.com/c58/marsdb) is a lightweight database for any kind of JS-environment (Browser, Node, NW.js, Electron), based on Meteor's minimongo. It uses Promises almost everywhere, carefully written on ES6. It can be in-memory (by default) or persisted with any kind of storage (LocalStorage implemented for now), and it's easy to implement your own storage manager (thanks to Promises).


## SQLite3

Use [better-sqlite3](https://www.npmjs.com/package/better-sqlite3) for NW.js compatibility. Example of it in use in an [Angular project](https://github.com/vatsalkgor/nw-sqlite3-error).


## LowDB
[LowDB](https://github.com/typicode/lowdb) is a flat JSON file database for Node. It uses Lo-Dash functional programming API.

## StoreDB

[StoreDB](https://github.com/djyde/StoreDB) is a local database **based on localStorage**. It allows the use of localStorage to store complex data by providing MongoDB-Style APIs and using concepts like `collection`, `document`, etc.

It is super simple and friendly to store data as below:

```javascript

//insert data
storedb('players').insert({"name":"Randy","sex":"male","score":20},function(err,result){
  if(!err){
    //do sth...
  } else //do sth...
})

//update data
storedb('players').update({"name":"Randy"},{"$inc":{"score":"10"}},function(err){
  if(!err){
    //do sth...
  } else //do sth...
})
```

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

**Be warned that for large data sets web storage is incredibly impractical, since the API is synchronous, there is no advanced indexing/queries (only one key-value store) and the value can only be a string.**



## Application Cache

HTML5 introduces application cache, which means that a web application is cached, and accessible without an internet connection.

Application cache gives an application three advantages:

* Offline browsing - users can use the application when they're offline
* Speed - cached resources load faster
* Reduced server load - the browser will only download updated/changed resources from the server

However, application cache is designed for browser use, for apps using node-webkit, it's less useful than the other two method, read [HTML5 Application Cache](http://www.w3schools.com/html/html5_app_cache.asp) if you want to use it.