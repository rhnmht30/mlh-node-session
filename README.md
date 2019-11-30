# Weather App using Node.js + Express + OpenWeather

## Here’s what it’ll look like: [Weather App](https://simple-nodejs-weather-app-irhhpddsku.now.sh/)

### **Pre-Project Setup**

Here’s what you’ll need:

- [OpenWeather](https://home.openweathermap.org/users/sign_up) account for a free weather information api.
- Once you sign up from an OpenWeather account, click on the **API Keys** option and copy the api key.
- Node.js: Visit the official [Node.js website](https://nodejs.org/en/) to download and install Node if you haven’t already.
- Install [VS-Code](https://code.visualstudio.com/) as your text editor with inbuilt terminal.

### **Project Setup**

- Create an empty folder named **weather-app**.
- Open up your vscode in the above folder, launch the terminal and run `npm init -y`
- A package.json file is created in your directory
- Change the code in your package.json file with the given code below:

  ```
  {
    "name": "nodejs-weather-app",
    "version": "1.0.0",
    "description": "",
    "main": "index.js",
    "scripts": {
     "start": "node index.js "
    },
    "keywords": [],
    "author": "",
    "license": "ISC"
  }
  ```

- Within our weather-app directory, create a file named `index.js`

### **Creating our Server (with Express JS)**

- To use express, install it in the terminal:
  `npm install express`
- Once installed, we’re going to copy the boilerplate Express starter app to our `index.js` file from the Express documentation:

```
const express = require('express')
const app = express()

app.get('/', function (req, res) {
res.send('Hello World!')
})

app.listen(3000, function () {
console.log('Example app listening on port 3000!')
})
```

> Above is an example of the simplest application that we can create with Express. First we require the express package that was just installed. Then, we create an instance named app by invoking Express.
> The app.get('/'... means we are specifically focusing on the root URL (/). If we visit the root URL, Express will respond with “Hello World!”.
> The app.listen(... shows we are creating a server that is listening on port 3000 for connections.

- We can test our server by running:
  `npm start`

- Now open your browser and visit: localhost:3000 and you should see Hello World!

### **Setting up the index view**

> Instead of responding with text when someone visits our root route, we’d like to respond with an HTML file. For this, we’ll be using EJS (Embedded JavaScript). EJS is a templating language.
> In order to use EJS in Express, we need to set up our template engine:
> A template engine enables you to use static template files in your application. At runtime, the template engine replaces variables in a template file with actual values, and transforms the template into an HTML file sent to the client. This approach makes it easier to design an HTML page.
> The short version is that EJS allows us to interact with variables and then dynamically create our HTML based on those variables!

- First, we’ll install ejs in the terminal:
  `npm install ejs`
- We can then set up our template engine with this line of code (just below our require statements) in our index.js file:

  ```
  ...
  app.set('view engine', 'ejs')
  ...
  ```

  > EJS is accessed by default in the views directory. So create a new folder named views in your directory. Within that `views folder`, add a file named index.ejs. Think of our `index.ejs` file as an HTML file for now.
  > Real quick, here’s what our file structure should look like thus far:

  ```
  |-- weather-app
     |-- views
       |-- index.ejs
     |-- package.json
     |-- index.js
  ```

  > Awesome, here’s a boilerplate for our index.ejs file. The html is just a form with one input for a city, and one submit button:

* `index.ejs`

```
  <!DOCTYPE html>
  <html>
    <head>
      <meta charset="utf-8">
      <title>Test</title>
      <link rel="stylesheet" type="text/css" href="/css/style.css">
      <link href='https://fonts.googleapis.com/css?family=Open+Sans:300' rel='stylesheet' type='text/css'>
    </head>
    <style>
          body {
        width: 800px;
        margin: 0 auto;
        font-family: 'Open Sans', sans-serif;
      }
      .container {
        width: 600px;
        margin: 0 auto;
      }
      fieldset {
        display: block;
        -webkit-margin-start: 0px;
        -webkit-margin-end: 0px;
        -webkit-padding-before: 0em;
        -webkit-padding-start: 0em;
        -webkit-padding-end: 0em;
        -webkit-padding-after: 0em;
        border: 0px;
        border-image-source: initial;
        border-image-slice: initial;
        border-image-width: initial;
        border-image-outset: initial;
        border-image-repeat: initial;
        min-width: -webkit-min-content;
        padding: 30px;
      }
      .ghost-input, p {
        display: block;
        font-weight:300;
        width: 100%;
        font-size: 25px;
        border:0px;
        outline: none;
        width: 100%;
        -webkit-box-sizing: border-box;
        -moz-box-sizing: border-box;
        box-sizing: border-box;
        color: #4b545f;
        background: #fff;
        font-family: Open Sans,Verdana;
        padding: 10px 15px;
        margin: 30px 0px;
        -webkit-transition: all 0.1s ease-in-out;
        -moz-transition: all 0.1s ease-in-out;
        -ms-transition: all 0.1s ease-in-out;
        -o-transition: all 0.1s ease-in-out;
        transition: all 0.1s ease-in-out;
      }
      .ghost-input:focus {
        border-bottom:1px solid #ddd;
      }
      .ghost-button {
        background-color: transparent;
        border:2px solid #ddd;
        padding:10px 30px;
        width: 100%;
        min-width: 350px;
        -webkit-transition: all 0.1s ease-in-out;
        -moz-transition: all 0.1s ease-in-out;
        -ms-transition: all 0.1s ease-in-out;
        -o-transition: all 0.1s ease-in-out;
        transition: all 0.1s ease-in-out;
      }
      .ghost-button:hover {
        border:2px solid #515151;
      }
      p {
        color: #E64A19;
      }
    </style>
    <body>
      <div class="container">
        <fieldset>
          <form action="/" method="post">
            <input name="city" type="text" class="ghost-input" placeholder="Enter a City" required>
            <input type="submit" class="ghost-button" value="Get Weather">
          </form>
        </fieldset>
      </div>
    </body>
  </html>
```

- Once you have the above code copied into your index.ejs file, you’re almost done! The final thing we need to do is replace our app.get code:

```
app.get('/', function (req, res) {
// OLD CODE
res.send('Hello World!')
})
```

- Above is the old code where we send the text ‘Hello World!’ to the client. Instead, we want to send our index.ejs file:

```
app.get('/', function (req, res) {
// NEW CODE
res.render('index');
})

```

- Instead of using res.send, we use res.render when working with a templating language. res.render will render our view, then send the equivalent HTML to the client.
- At this point, we can test again by running:
  `npm start`

- Now open your browser and visit: localhost:3000 and you should see our index.ejs file being displayed!

### **Setting up our POST Route**

By now, your index.js file should look like this:

```

const express = require('express');
const app = express()

app.use(express.static('public'));
app.set('view engine', 'ejs')

app.get('/', function (req, res) {
  res.render('index');
})
app.listen(3000, function () {
  console.log('Example app listening on port 3000!')
})
```

- We have one get route, and then we create our server. However, for our application to work, we need a post route as well. If you look at our `index.ejs` file, you can see that our form is submitting a post request to the / route:

  ```
    <form action="/" method="post">
  ```

- Now that we know where our form is posting, we can set up the route! A post request looks just like a get request, with one minor change:

  ```
  ...
  app.post('/', function (req, res) {
  res.render('index');
  })
  ...
  ```

- But instead of just responding with the same html template, lets access the name of the city the user typed in as well. For this we need to use an Express Middleware (functions that have access to the request and response bodies) in order to preform more advanced tasks.

- We’re going to make use of the body-parser middleware. body-parser allows us to make use of the key-value pairs stored on the req-body object. In this case, we’ll be able to access the city name the user typed in on the client side.
- To use body-parser, we must install it first:
  `npm install body-parser`
- Once installed, we can require it, and then make use of our middleware with the following line of code in our `index.js`
  ```
  ...
  const bodyParser = require('body-parser');
  // ...
  // ...
  app.use(bodyParser.urlencoded({ extended: true }));
  ...
  ```
- For the scope of this project, it’s not necessary you understand exactly how that line of code works. Just know that by using body-parser we can make use of the req.body object.
- Finally, we can now update our post request to log the value of ‘city’ to the console.

  ```
  ...
  app.post('/', function (req, res) {
  res.render('index');
  console.log(req.body.city);
  })
  ...
  ```

- Lets test it! --> `npm start`
- Now open your browser and visit: localhost:3000, type a city name into the field and hit enter!
- If you go back to your command prompt, you should see the city name displayed in the prompt! Awesome, you’ve now successfully passed data from the client to the server!
  If you can’t get it to work, here’s what your index.js file should look like now:

  ```
  const express = require('express');
  const bodyParser = require('body-parser');
  const app = express()

  app.use(express.static('public'));
  app.use(bodyParser.urlencoded({ extended: true }));
  app.set('view engine', 'ejs')

  app.get('/', function (req, res) {
    res.render('index');
  })
  app.post('/', function (req, res) {
    console.log(req.body.city);
    res.render('index');
  })
  app.listen(3000, function () {
    console.log('Example app listening on port 3000!')
  })
  ```

### **Finishing app.post**

- To finish up this project, you’ll need the code below.
- What were going to do is make a request to the OpenWeatherMap API in our app.post request. For this we'll need a library called **request** which can handle our api calls. Install it just like others: `npm install request`
- Finally here’s what the code looks like:

```
  const request = require('request');
  const apiKey = '*****************';
  //...
  //...
  app.post('/', function (req, res) {
    let city = req.body.city;
    let url = `http://api.openweathermap.org/data/2.5/weather?q=${city}&units=imperial&appid=${apiKey}`
  request(url, function (err, response, body) {
      if(err){
        res.render('index', {weather: null, error: 'Error, please try again'});
      } else {
        let weather = JSON.parse(body)
        if(weather.main == undefined){
          res.render('index', {weather: null, error: 'Error, please try again'});
        } else {
          let weatherText = `It's ${weather.main.temp} degrees in ${weather.name}!`;
          res.render('index', {weather: weatherText, error: null});
        }
      }
    });
  })
```

- Woah. That’s a lot of new code. Lets break it all down:

> 1.  Setting up our URL:
>     The first thing we do when we receive the post request is grab the city off of req.body. Then we create a url string that we’ll use to access the OpenWeatherMap API with

```
app.post('/', function (req, res) {
    let city = req.body.city;
    let url = `http://api.openweathermap.org/data/2.5/weather?q=${city}&units=imperial&appid=${apiKey}
```

> 2. Make our API call:
>    Now that we have our URL, we can make our API call. When we receive our callback, the first thing we’re going to do is check for an error. If we have an error, we’re going to render our index page. But notice that I’ve also added in a second argument. res.render has an optional second argument — an object where we can specify properties to be handled by our view ( index.ejs ).
>    In this instance, I’ve added an error string:

```
    request(url, function (err, response, body) {
    if(err){
    res.render('index', {weather: null, error: 'Error, please try again'});
```

> 3.  Display the weather:
>     Now that we know we have no API error, we can parse our JSON into a usable JavaScript object.
>     The first thing we’ll do is check to see if `weather.main == undefined` . The only reason why this would be undefined, is if our user input a string that isn’t a city (‘3’, ‘afefaefefe’, etc.). In this instance, we’ll render the index view, and we’ll also pass back an error.
>     If `weather.main != undefined`, then we can finally send back the weather to the client! We’ll create a string that clarifies what the weather is, and send that back with the index view.

```
     } else {
     let weather = JSON.parse(body)
     if(weather.main == undefined){
     res.render('index', {weather: null, error: 'Error, please try again'});
     } else {
     let weatherText = `It's ${weather.main.temp} degrees in ${weather.name}!`;
     res.render('index', {weather: weatherText, error: null});
     }
     }
```

> Using EJS
> There’s only one thing left to do at this point… Make use of all those variables we sent back with our res.render call. These variables aren’t available on the client, this is where we finally get to use EJS. There are three possible scenarios that we have in our code:
> `{weather: null, error: null}` > `{weather: null, error: ‘Error, please try again’}`
> {weather: weatherText, error: null}
> We need to make two simple changes to our index.ejs to handle these three scenarios. Here’s the code, then I’ll walk you through it:

```
<% if(weather !== null){ %>

  <p><%= weather %></p>

<% } %>
<% if(error !== null){ %>

<p><%= error %></p>
<% } %>
```

- It helps to remember that EJS stands for Embedded JavaScript. With that in mind, EJS has opening and closing brackets: `<% CODE HERE %>` Anything between the brackets is executed.

- If the opening bracket also includes an equal sign: `<%= CODE HERE ADDS HTML %>`, then that code will add HTML to the result.
- If you look at our EJS that we’ve added, we’re testing to see if either of our weather, or error variables are null. If they’re both null, nothing happens. However, if one isn’t null (i.e. it has a value), then we will add a paragraph to the screen with the value of the respective variable.index.ejs

- Here’s what your index.ejs should look like:

```
<!DOCTYPE html>
<html>
  <head>
    <meta charset="utf-8">
    <title>Test</title>
    <link href='https://fonts.googleapis.com/css?family=Open+Sans:300' rel='stylesheet' type='text/css'>

  </head>
  <style>
          body {
        width: 800px;
        margin: 0 auto;
        font-family: 'Open Sans', sans-serif;
      }
      .container {
        width: 600px;
        margin: 0 auto;
      }
      fieldset {
        display: block;
        -webkit-margin-start: 0px;
        -webkit-margin-end: 0px;
        -webkit-padding-before: 0em;
        -webkit-padding-start: 0em;
        -webkit-padding-end: 0em;
        -webkit-padding-after: 0em;
        border: 0px;
        border-image-source: initial;
        border-image-slice: initial;
        border-image-width: initial;
        border-image-outset: initial;
        border-image-repeat: initial;
        min-width: -webkit-min-content;
        padding: 30px;
      }
      .ghost-input, p {
        display: block;
        font-weight:300;
        width: 100%;
        font-size: 25px;
        border:0px;
        outline: none;
        width: 100%;
        -webkit-box-sizing: border-box;
        -moz-box-sizing: border-box;
        box-sizing: border-box;
        color: #4b545f;
        background: #fff;
        font-family: Open Sans,Verdana;
        padding: 10px 15px;
        margin: 30px 0px;
        -webkit-transition: all 0.1s ease-in-out;
        -moz-transition: all 0.1s ease-in-out;
        -ms-transition: all 0.1s ease-in-out;
        -o-transition: all 0.1s ease-in-out;
        transition: all 0.1s ease-in-out;
      }
      .ghost-input:focus {
        border-bottom:1px solid #ddd;
      }
      .ghost-button {
        background-color: transparent;
        border:2px solid #ddd;
        padding:10px 30px;
        width: 100%;
        min-width: 350px;
        -webkit-transition: all 0.1s ease-in-out;
        -moz-transition: all 0.1s ease-in-out;
        -ms-transition: all 0.1s ease-in-out;
        -o-transition: all 0.1s ease-in-out;
        transition: all 0.1s ease-in-out;
      }
      .ghost-button:hover {
        border:2px solid #515151;
      }
      p {
        color: #E64A19;
      }
  </style>
  <body>
    <div class="container">
      <fieldset>
        <form action="/" method="post">
          <input name="city" type="text" class="ghost-input" placeholder="Enter a City" required>
          <input type="submit" class="ghost-button" value="Get Weather">
        </form>
        <% if(weather !== null){ %>
          <p><%= weather %></p>
        <% } %>

        <% if(error !== null){ %>
          <p><%= error %></p>
        <% } %>
      </fieldset>
    </div>
  </body>
</html>
```

- Run your code --> `npm start`
- Now open your browser and visit: localhost:3000, type a city name into the field and hit enter! You should see the weather appear on your screen!

- You just built a website that makes API calls and responds to the client in real time!
