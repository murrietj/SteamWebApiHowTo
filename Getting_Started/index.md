---
layout: default
title: Getting Started
---

# Register for an API key
As is the case with almost every web API, a unique key is required to make calls to methods within the API. This key serves as a way to identify who is making the API calls. To obtain a key, simply [fill out this form](http://steamcommunity.com/dev/apikey). Note that you must first have a Steam account to link with your key. You can sign in with an existing account or create a new one. Also note that you are limited to 100,000 calls per day with your key, as described in the [API Terms of Use](https://steamcommunity.com/dev/apiterms). For this reason, you will want to avoid sharing your key.

# API Call Format
API methods are called through a GET or POST (call specific) request made to http://api.steampowered.com/. We will only cover GET requests in this guide as that is how the great majority of the methods in the API are made. The URL request string is formatted as follows:

<p style="font-size:12px">http://api.steampowered.com/<font color="orange">&lt;interface name&gt;</font>/<font color="lightblue">&lt;method name&gt;</font>/v<font color="lightgreen">&lt;version&gt;</font>/?key=<font color="yellow">&lt;api key&gt;</font>&format=<font color="pink">&lt;format&gt;</font></p>

## <font color="orange">Interface Name
Steam categorizes their API into "Interfaces" which groups together related API calls. This is where you'll provide the name of the interface that the specific method you want to call belongs to.

## <font color="lightblue">Method Name
The name of the method you want to call.

## <font color="lightgreen">Version
The version of the method you want to call.

## <font color="yellow">Api Key
The unique API key that serves as an identifier for the caller.

## <font color="pink">Format
The format that you want to receive the requested data. The availabe formats are:
- JSON
- XML
- VDF

You can find more information about the data formats returned in the [Steam Web API Wiki](https://developer.valvesoftware.com/wiki/Steam_Web_API#Formats).
If you do not specify a format, the API will default to JSON.

## Additional Parameters
Any additional parameters (required or optional) expected by the method you want to call should be appended to the end of the url delimited by the '&' character.

# Setting up a server
A quite frustrating feature of the Steam Web API is that it does not allow cross-origin requests. Because of this, we will need to set up our own server to handle the requests. We will do so by using [Node.js](https://nodejs.org/) along with the [Express](http://expressjs.com/) and [Request](https://github.com/request/request) packages. To install these packages, you will need [npm](https://www.npmjs.com/) (node package manager). Once you have Node.js and npm, you can install the packages by opening the console and navigating to the working directory of your server and entering the command: `npm install express request`.

## Server code
Create a new empty javascript file named **server.js**. To make use of the installed packages in our server, we need to include these modules in our server.js file. We can do this by adding the following lines to the top of the file.

```js
var express = require('express');
var request = require('request');
```

[Express](http://expressjs.com/) provides an easy way to handle HTTP requests sent to our server and [Request](https://github.com/request/request) provides and easy way for us to send HTTP requests to another server, in this case the [Steam Web API](http://steamcommunity.com/dev) server. Now we need to create a server object so that we can configure our server and run it at the end with its`.listen()`method.

```js
var app = express();
```

To handle GET requests made to our server, we can now use the`.get()`method. Let's create an event handler for when a GET request is made to our root-level URL.

```js
app.get('/', function(req, res){
  //insert code here
})
```

Now when our server receives a GET request to its root-level URL, the code inside the function parameter will execute. We can do whatever processing we want before sending the caller the requested data as an argument in`res.send()`.

Now we want to make an HTTP request to the [Steam Web API](http://steamcommunity.com/dev) inside the function argument of our GET request handler. Inside the function parameter, let's add our code to make this request.

```js
app.get('/', function(req, res){
  //set the url for the request
  var url = //assign Steam Web API URL here;
  //make the GET request
  request.get(url, function(error, steamHttpResponse, steamHttpBody){
    //After receiving a response from the API, sent the body of the response
    //as a response to our client.
    res.setHeader('Content-Type', 'application/json');
    res.send(steamHttpBody);
  });
});
```
**NOTE: Make sure to assign the URL of the desired request to to the`url`variable.**

Finally, we just need to run our server with the`.listen()` method. We do this by setting a`port`attribute on our server object,`app`, and then calling`.lisen()`with the port that we set.

```js
app.set('port', 1234);
app.listen(app.get('port'), function(){
  console.log('Express started on port:' + app.get('port') + '; press Ctrl-C to terminate.');
});

```

That's it!! Our server is now ready to start listening for requests. To start our server, enter`node server.js`in the console within the working directory of our server. To test if your server is working correctly, open navigate to [http://localhost:1234/](http://localhost:1234/). You should see a the data you requested in JSON format. 

In the following section, we'll go through some examples of calls to the api using our server and examine the returned data.