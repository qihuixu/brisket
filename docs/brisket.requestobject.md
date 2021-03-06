Brisket Request Object
======================

The second to last parameter of every route handler is a Brisket request object - a normalized view of the current request.

## Documentation Index

* [Accessing the Request Object](#accessing-the-request-object)
* [What's in the Request Object?](#whats-in-the-request-object)

## Accessing the Request Object

### Second to last parameter of a route handler

```js
var BookRouter = Brisket.Router.extend({

  routes: {
    'books': 'books',
    'books/:id': 'book'
  },

  books: function(layout, request, response) {
    console.log(request.host); // 'example.com:8080'

    return new BookView();
  },

  book: function(id, layout, request, response) {
    var book = new Book({ id: id });

    console.log(request.isNotClick); // true/false

    return book.fetch()
      .then(function() {
        return new BookView({ model: book });
      });
  }

});
```

### Second parameter of router callbacks

```js
var Router = Brisket.Router.extend({

  onRouteStart: function(layout, request, response) {
    console.log(request.isFirstRequest); // true/false
  },

  onRouteComplete: function(layout, request, response) {
    console.log(request.requestId); // 1, 2, 3,...
  }

});
```


## What's in the Request Object?
Here is a listing of all the data provided by the Brisket request object.

### request.host
The host of the url including the port e.g. 'example.com:8080'.

### request.hostname
The host of the url 'example.com'.

### request.isFirstRequest
A boolean that let's you know if this is the first request to your Brisket application e.g. the initial request for your application, a page refresh.

### request.isNotClick
A boolean that let's you know if the current request was **NOT** triggered by a user click e.g. forward/back button press.

### request.path
The path of the URL with a preceding '/' e.g. '/path/to/route'.

### request.applicationPath
The applicationPath of the request. This value is always the same regardless of where the application is deployed. It does NOT have a preceding slash '/' e.g. 'path/to/route'.

### request.protocol
The protocol scheme of the URL, **NOT** including the ':' e.g. 'http', 'https'.

### request.rawQuery
The raw query string. It is the same as extracting query string express request.originalUrl.

### request.query
An object with the parsed values from the query string. It is the same as express request.query.

### request.referrer
The full URI of the previous request e.g. previous page, previous click within your application.

### request.requestId
The unique identifier for each request to your application.

### request.userAgent
The user agent string from the user's browser.

### request.environmentConfig
The [environmentConfig](brisket.createserver.md#environmentconfig) that you use to create your Brisket server.

### request.cookies
The user's cookies for the current domain. The api looks the same as express' [request.cookies](http://expressjs.com/4x/api.html#req.cookies).

**Note: To enable cookies, you have to use the cookie parser middleware as specified by the express documentation.**

**Another note: Please be advised that if you use a downstream cache (e.g. CDN), you should add any cookie values that are used to make rendering choices into the url's cache key.**

### request.onComplete(fn)
Specify a callback to execute when a request completes (i.e. is visible to the user).
