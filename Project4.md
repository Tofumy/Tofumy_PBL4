# Project 4

**Step 1** - Install NodeJs
---

The below cmdlet update Ubuntu

`sudo apt update`

![screenshot](https://github.com/Tofumy/Tofumy_PBL4/blob/main/sudo-update.png)


Upgraded ubuntu

`sudo apt upgrade`

![screenshot](https://github.com/Tofumy/Tofumy_PBL4/blob/main/sudo-upgrade.png)

Installed NodeJS

`sudo apt install -y nodejs`

![screenshot](https://github.com/Tofumy/Tofumy_PBL4/blob/main/sudo-apt-install-mongodb.png)

**Step 2** : Install MongoDB

`sudo apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv 0C49F3730359A14518585931BC711F9BA15703C6`


`echo "deb [ arch=amd64 ] https://repo.mongodb.org/apt/ubuntu trusty/mongodb-org/3.4 multiverse" | sudo tee /etc/apt/sources.list.d/mongodb-org-3.4.list`

![screenshot](https://github.com/Tofumy/Tofumy_PBL4/blob/main/sudo-apt-key-adv.png)



To install the MongoDB, we run the below:

`sudo apt install -y mongodb`

![screenshot](https://github.com/Tofumy/Tofumy_PBL4/blob/main/sudo-apt-install-mongodb.png)


Started the mongodb server using the below:

`sudo service mongodb start`

We then verified if the service is up and running:

`sudo systemctl status mongodb`

![screenshot](https://github.com/Tofumy/Tofumy_PBL4/blob/main/sudo-service-mongodb-start-status.png)


Installed a node package manager

`sudo apt install -y npm`

![screenshot](https://github.com/Tofumy/Tofumy_PBL4/blob/main/sudo-apt-install-npm.png)

Installed a 'body-parser' package to help process JSON files passed in requests to server

`sudo npm install body-parser`

![screenshot](https://github.com/Tofumy/Tofumy_PBL4/blob/main/sudo-npm-install-body-parser.png)

Created a folder named ‘Books’

`mkdir Books && cd Books`

![screenshot](https://github.com/Tofumy/Tofumy_PBL4/blob/main/mkdir-books.png)


In the Books directory, an npm project was initialized

`npm init`

![screenshot](https://github.com/Tofumy/Tofumy_PBL4/blob/main/npm-init.png)

Added a file to the directory 'server.js'

`vi server.js`

Below is the code added to the file

``` javascript

var express = require('express');
var bodyParser = require('body-parser');
var app = express();
app.use(express.static(__dirname + '/public'));
app.use(bodyParser.json());
require('./apps/routes')(app);
app.set('port', 3300);
app.listen(app.get('port'), function() {
    console.log('Server up: http://localhost:' + app.get('port'));
});

```

![screenshot](https://github.com/Tofumy/Tofumy_PBL4/blob/main/server.js.png)



**Step 3** Install Express and Set up Routes to the server

We installed the mongoose package for express framework

`sudo npm install express mongoose`


![screenshot](https://github.com/Tofumy/Tofumy_PBL4/blob/main/sudo-npm-install-mongoose.png)

We created a folder "apps" and in the folder, we created a file "routes.js"

`mkdir apps && cd apps`

`vi routes.js`

We added the below code to the 'routes.js' file

``` javascript

var Book = require('./models/book');
module.exports = function(app) {
  app.get('/book', function(req, res) {
    Book.find({}, function(err, result) {
      if ( err ) throw err;
      res.json(result);
    });
  }); 
  app.post('/book', function(req, res) {
    var book = new Book( {
      name:req.body.name,
      isbn:req.body.isbn,
      author:req.body.author,
      pages:req.body.pages
    });
    book.save(function(err, result) {
      if ( err ) throw err;
      res.json( {
        message:"Successfully added book",
        book:result
      });
    });
  });
  app.delete("/book/:isbn", function(req, res) {
    Book.findOneAndRemove(req.query, function(err, result) {
      if ( err ) throw err;
      res.json( {
        message: "Successfully deleted the book",
        book: result
      });
    });
  });
  var path = require('path');
  app.get('*', function(req, res) {
    res.sendfile(path.join(__dirname + '/public', 'index.html'));
  });
};

```

![screenshot](https://github.com/Tofumy/Tofumy_PBL4/blob/main/routes.js.png)


We created another folder 'models' in the 'apps' folder and created a file called 'book.js'

`mkdir models && cd models`

`vi book.js`

We pasted the below code in the 'book.js' file

``` javascript

var mongoose = require('mongoose');
var dbHost = 'mongodb://localhost:27017/test';
mongoose.connect(dbHost);
mongoose.connection;
mongoose.set('debug', true);
var bookSchema = mongoose.Schema( {
  name: String,
  isbn: {type: String, index: true},
  author: String,
  pages: Number
});
var Book = mongoose.model('Book', bookSchema);
module.exports = mongoose.model('Book', bookSchema);

```

![screenshot](https://github.com/Tofumy/Tofumy_PBL4/blob/main/book.js.png)



**Step 4** - Access the routes with AngularJS

We changed to the 'Books' directory

`cd ../..`

Then created a folder 'public'

`mkdir public && cd public`

Created a file 'script.js'

`vi script.js`

then copied and pasted the below code in the 'script.js'

``` javascript

var app = angular.module('myApp', []);
app.controller('myCtrl', function($scope, $http) {
  $http( {
    method: 'GET',
    url: '/book'
  }).then(function successCallback(response) {
    $scope.books = response.data;
  }, function errorCallback(response) {
    console.log('Error: ' + response);
  });
  $scope.del_book = function(book) {
    $http( {
      method: 'DELETE',
      url: '/book/:isbn',
      params: {'isbn': book.isbn}
    }).then(function successCallback(response) {
      console.log(response);
    }, function errorCallback(response) {
      console.log('Error: ' + response);
    });
  };
  $scope.add_book = function() {
    var body = '{ "name": "' + $scope.Name + 
    '", "isbn": "' + $scope.Isbn +
    '", "author": "' + $scope.Author + 
    '", "pages": "' + $scope.Pages + '" }';
    $http({
      method: 'POST',
      url: '/book',
      data: body
    }).then(function successCallback(response) {
      console.log(response);
    }, function errorCallback(response) {
      console.log('Error: ' + response);
    });
  };
});

```

![screenshot](https://github.com/Tofumy/Tofumy_PBL4/blob/main/script.js.png)


We then created an 'index.html' in the 'public' directory

`vi index.html`

Then pasted the below code:

``` html

<!doctype html>
<html ng-app="myApp" ng-controller="myCtrl">
  <head>
    <script src="https://ajax.googleapis.com/ajax/libs/angularjs/1.6.4/angular.min.js"></script>
    <script src="script.js"></script>
  </head>
  <body>
    <div>
      <table>
        <tr>
          <td>Name:</td>
          <td><input type="text" ng-model="Name"></td>
        </tr>
        <tr>
          <td>Isbn:</td>
          <td><input type="text" ng-model="Isbn"></td>
        </tr>
        <tr>
          <td>Author:</td>
          <td><input type="text" ng-model="Author"></td>
        </tr>
        <tr>
          <td>Pages:</td>
          <td><input type="number" ng-model="Pages"></td>
        </tr>
      </table>
      <button ng-click="add_book()">Add</button>
    </div>
    <hr>
    <div>
      <table>
        <tr>
          <th>Name</th>
          <th>Isbn</th>
          <th>Author</th>
          <th>Pages</th>

        </tr>
        <tr ng-repeat="book in books">
          <td>{{book.name}}</td>
          <td>{{book.isbn}}</td>
          <td>{{book.author}}</td>
          <td>{{book.pages}}</td>

          <td><input type="button" value="Delete" data-ng-click="del_book(book)"></td>
        </tr>
      </table>
    </div>
  </body>
</html>

```

![screenshot](https://github.com/Tofumy/Tofumy_PBL4/blob/main/index.html.png)


Changed the directory back to the 'books' directory

`cd ..`

Started the node server

`node server.js`

![screenshot](https://github.com/Tofumy/Tofumy_PBL4/blob/main/node-server.js.png)

We created a custom TCP Port 3300 in the security group of the AWS EC2 instance

![screenshot](https://github.com/Tofumy/Tofumy_PBL4/blob/main/security-inboundrule.png)


We did a test to see what the server returns locally:

`curl -s http://localhost:3300`

![screenshot](https://github.com/Tofumy/Tofumy_PBL4/blob/main/curl-localhost.png)


We tried to access the app on the browser and it works fine

![screenshot](https://github.com/Tofumy/Tofumy_PBL4/blob/main/book-register.png)










