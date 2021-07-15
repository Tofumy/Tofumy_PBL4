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


![screenshot](https://github.com/Tofumy/Tofumy_PBL4/blob/main/server.js.png)

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

![screenshot](https://github.com/Tofumy/Tofumy_PBL4/blob/main/server.js.png)


We created another folder 'models' in the 'apps' folder and created a file called 'book.js'

`mkdir models && cd models`

`vi book.js`

We pasted the below code in the 'book.js' file




