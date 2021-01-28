# Node.js Interview Questions

* *[NodeJS APIs - Latest v15.x](https://nodejs.org/dist/latest-v15.x/docs/api/)*

### Questions

#### 1. ***What is Node.js?***
Node.js is an open-source server side runtime environment built on Chrome\'s V8 JavaScript engine. It provides an event driven, non-blocking (asynchronous) I/O and cross-platform runtime environment for building highly scalable server-side applications using JavaScript.

#### 2. ***What are the data types in Node.js?***
*Primitive Types*

* String
* Number
* Boolean
* Undefined
* Null
* RegExp

* `Buffer`: Node.js includes an additional data type called Buffer (not available in browser\'s JavaScript). Buffer is mainly used to store binary data, while reading from a file or receiving packets over the network.

#### 3. ***How do Node.js works?***
Node is completely event-driven. Basically the server consists of one thread processing one event after another.

A new request coming in is one kind of event. The server starts processing it and when there is a blocking IO operation, it does not wait until it completes and instead registers a callback function. The server then immediately starts to process another event (maybe another request). When the IO operation is finished, that is another kind of event, and the server will process it (i.e. continue working on the request) by executing the callback as soon as it has time.

So the server never needs to create additional threads or switch between threads, which means it has very little overhead. If you want to make full use of multiple hardware cores, you just start multiple instances of node.js

Node JS Platform does not follow Request/Response Multi-Threaded Stateless Model. It follows Single Threaded with Event Loop Model. Node JS Processing model mainly based on Javascript Event based model with Javascript callback mechanism.  

#### 4. ***How do Node.js works?***

<p align="center">
  <img src="https://github.com/learning-zone/nodejs-interview-questions/blob/master/assets/event-loop.png" alt="Node Architecture" width="800px" />
</p>
  
**Single Threaded Event Loop Model Processing Steps:**

* Clients Send request to Web Server.
* Node JS Web Server internally maintains a Limited Thread pool to provide services to the Client Requests.
* Node JS Web Server receives those requests and places them into a Queue. It is known as “Event Queue”.
* Node JS Web Server internally has a Component, known as “Event Loop”. Why it got this name is that it uses indefinite loop to receive requests and process them. 
* Event Loop uses Single Thread only. It is main heart of Node JS Platform Processing Model.
* Even Loop checks any Client Request is placed in Event Queue. If no, then wait for incoming requests for indefinitely.
* If yes, then pick up one Client Request from Event Queue
    * Starts process that Client Request
    * If that Client Request Does Not requires any Blocking IO Operations, then process everything, prepare response and send it back to client.
    * If that Client Request requires some Blocking IO Operations like interacting with Database, File System, External Services then it will follow different approach
        * Checks Threads availability from Internal Thread Pool
        * Picks up one Thread and assign this Client Request to that thread.
        * That Thread is responsible for taking that request, process it, perform Blocking IO operations, prepare response and send it back to the Event Loop
        * Event Loop in turn, sends that Response to the respective Client.
        
#### 5. ***What is an error-first callback?***
Error-first callbacks are used to pass errors and data. The first argument is always an error object that the programmer has to check if something went wrong. Additional arguments are used to pass data.
```javascript
fs.readFile( "file.json", function ( err, data ) {
  if ( err ) {
    // handle the error
    console.error( err );
  }
    // use the data object
    console.log( data );
});
```

#### 6. ***What's the difference between operational and programmer errors?***
Operation errors are not bugs, but problems with the system, like request timeout or hardware failure.
On the other hand programmer errors are actual bugs.


#### 7. ***What is a stub?***
Stubs are functions/programs that simulate the behaviours of components/modules. Stubs provide canned answers to function calls made during test cases. Also, you can assert on with what these stubs were called.

A use-case can be a file read, when you do not want to read an actual file:
```javascript
var fs = require('fs');

var readFileStub = sinon.stub(fs, 'readFile', function (path, cb) {
  return cb(null, 'filecontent');
});

expect(readFileStub).to.be.called;
readFileStub.restore();
```

#### 8. ***How can you secure your HTTP cookies against XSS attacks?***
+  When the web server sets cookies, it can provide some additional attributes to make sure the cookies won\'t be accessible by using malicious JavaScript. One such attribute is HttpOnly.

```javascript
Set-Cookie: [name]=[value]; HttpOnly
```

HttpOnly makes sure the cookies will be submitted only to the domain they originated from.

+ The "Secure" attribute can make sure the cookies are sent over secured channel only.

```javascript
Set-Cookie: [name]=[value]; Secure
```

+ The web server can use X-XSS-Protection response header to make sure pages do not load when they detect reflected cross-site scripting (XSS) attacks.

```javascript
X-XSS-Protection: 1; mode=block
```

+ The web server can use HTTP Content-Security-Policy response header to control what resources a user agent is allowed to load for a certain page. It can help to prevent various types of attacks like Cross Site Scripting (XSS) and data injection attacks.

```javascript
Content-Security-Policy: default-src 'self' *.http://sometrustedwebsite.com
```

#### 9. ***What is REPL? What purpose it is used for?***
REPL stands for Read Eval Print Loop and it represents a computer environment like a Windows console or Unix/Linux shell where a command is entered and the system responds with an output in an interactive mode. Node.js or Node comes bundled with a REPL environment. It performs the following tasks −

- Read − Reads user's input, parses the input into JavaScript data-structure, and stores in memory.
- Eval − Takes and evaluates the data structure.
- Print − Prints the result.
- Loop − Loops the above command until the user presses ctrl-c twice.

The REPL feature of Node is very useful in experimenting with Node.js codes and to debug JavaScript codes.
Simple Expression:
```javascript
$ node
> 10 + 20
30
> 10 + ( 20 * 30 ) - 40
570
>
```

#### 10. ***What is the difference between Asynchronous and Non-blocking?***
**Asynchronous VS Non-Blocking**  

1) Asynchronous does not respond immediately, While Nonblocking responds immediately if the data is available and if not that simply returns an error.
2) Asynchronous improves the efficiency by doing the task fast as the response might come later, meanwhile, can do complete other tasks. Nonblocking does not block any execution and if the data is available it retrieves the information quickly.
3) Asynchronous is the opposite of synchronous while nonblocking I/O is the opposite of blocking. They both are fairly similar but they are also different as asynchronous is used with a broader range of operations while nonblocking is mostly used with I/O.
