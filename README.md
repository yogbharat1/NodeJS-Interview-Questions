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
        
#### 4. ***Why is Node.js Single-threaded?***
Node.js is single-threaded for async processing. By doing async processing on a single-thread under typical web loads, more performance and scalability can be achieved instead of the typical thread-based implementation.

#### 5. ***How would you define the term I/O?***
- The term I/O is used to describe any program, operation, or device that transfers data to or from a medium and to or from another medium
- Every transfer is an output from one medium and an input into another. The medium can be a physical device, network, or files within a system

#### 6. ***What is npm in Node.js??***
NPM stands for Node Package Manager. It provides following two main functionalities.

* It works as an Online repository for node.js packages/modules which are present at <nodejs.org>.
* It works as Command line utility to install packages, do version management and dependency management of Node.js packages.
NPM comes bundled along with Node.js installable. We can verify its version using the following command-

```bash
npm --version
```

NPM helps to install any Node.js module using the following command.

```bash
npm install <Module Name>
```

For example, following is the command to install Node.js web framework module called express-

```bash
npm install express
```

#### 7. ***Which database is more popularly used with Node.js?***
MongoDB is the most common database used with Node.js. It is a NoSQL, cross-platform, document-oriented database that provides high performance, high availability, and easy scalability.

#### 8. ***What are the pros and cons of Node.js?***

**1. Pros**
- Fast processing and an event-based model
- Uses JavaScript, which is well-known amongst developers
- Node Package Manager has over 50,000 packages that provide the functionality to an application
- Best suited for streaming huge amounts of data and I/O intensive operations

**1. Cons**
- Not suitable for heavy computational tasks
- Using callback is complex since you end up with several nested callbacks
- Dealing with relational databases is not a good option for Node.js
- Since Node.js is single-threaded, CPU intensive tasks are not its strong suit


#### 9. ***What are globals in Node.js?***
There are three keywords in Node.js which constitute as Globals. These are Global, Process, and Buffer.

**1. Global**

The Global keyword represents the global namespace object. It acts as a container for all other `global` objects. If we type `console.log(global)`, it will print out all of them.

An important point to note about the global objects is that not all of them are in the global scope, some of them fall in the module scope. So, it is wise to declare them without using the var keyword or add them to Global object.

Variables declared using the var keyword become local to the module whereas those declared without it get subscribed to the global object.

**2. Process**

It is also one of the global objects but includes additional functionality to turn a synchronous function into an async callback. There is no boundation to access it from anywhere in the code. It is the instance of the EventEmitter class. And each node application object is an instance of the Process object.

It primarily gives back the information about the application or the environment.

* `<process.execPath>` – to get the execution path of the Node app.
* `<process.Version>` – to get the Node version currently running.
* `<process.platform>` – to get the server platform.

Some of the other useful Process methods are as follows.

* `<process.memoryUsage>` – To know the memory used by Node application.
* `<process.NextTick>` – To attach a callback function that will get called during the next loop. It can cause a delay in executing a function.

**3. Buffer**

The Buffer is a class in Node.js to handle binary data. It is similar to a list of integers but stores as a raw memory outside the V8 heap.

We can convert JavaScript string objects into Buffers. But it requires mentioning the encoding type explicitly.

* `<ascii>` – Specifies 7-bit ASCII data.
* `<utf8>` – Represents multibyte encoded Unicode char set.
* `<utf16le>` – Indicates 2 or 4 bytes, little endian encoded Unicode chars.
* `<base64>` – Used for Base64 string encoding.
* `<hex>` – Encodes each byte as two hexadecimal chars.

Here is the syntax to use the Buffer class.

```javascript
var buffer = new Buffer(string, [encoding]);
```
        
#### 10. ***What is an error-first callback?***
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

#### 11. ***What's the difference between operational and programmer errors?***
Operation errors are not bugs, but problems with the system, like request timeout or hardware failure.
On the other hand programmer errors are actual bugs.


#### 12. ***What is a stub?***
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

#### 13. ***How can you secure your HTTP cookies against XSS attacks?***
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

#### 14. ***What is REPL? What purpose it is used for?***
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

#### 15. ***What is the difference between Asynchronous and Non-blocking?***
**Asynchronous VS Non-Blocking**  

1) Asynchronous does not respond immediately, While Nonblocking responds immediately if the data is available and if not that simply returns an error.
2) Asynchronous improves the efficiency by doing the task fast as the response might come later, meanwhile, can do complete other tasks. Nonblocking does not block any execution and if the data is available it retrieves the information quickly.
3) Asynchronous is the opposite of synchronous while nonblocking I/O is the opposite of blocking. They both are fairly similar but they are also different as asynchronous is used with a broader range of operations while nonblocking is mostly used with I/O.

#### 16. ***What is EventEmitter in Node.js?***
The EventEmitter is a module that facilitates communication/interaction between objects in Node. EventEmitter is at the core of Node asynchronous event-driven architecture.
All objects that emit events are members of EventEmitter class. When the EventEmitter object emits an event, all of the functions attached to that specific event are called synchronously. All values returned by the called listeners are ignored and will be discarded.

#### 17. ***How many types of streams are present in node.js?***
Streams are objects that let you read data from a source or write data to a destination in continuous fashion.
There are four types of streams

* **Readable** − Stream which is used for read operation.
* **Writable** − Stream which is used for write operation.
* **Duplex** − Stream which can be used for both read and write operation.
* **Transform** − A type of duplex stream where the output is computed based on input. 

#### 18. ***What is chaining process in Node.js?***
It is an approach to connect the output of one stream to the input of another stream, thus creating a chain of multiple stream operations.

#### 19. ***What is the use of DNS module in Node.js?***
DNS is a node module used to do name resolution facility which is provided by the operating system as well as used to do an actual DNS lookup.
```javascript
// Include 'dns' module and create its object 
const dns = require('dns'); 
  
const website = 'adfs.alfalaval.com'; 
// Call to lookup function of dns 
dns.lookup(website, (err, address, family) => { 
  console.log('address of %s is %j family: IPv%s', website, address, family);     // address of adfs.alfalaval.com is "212.39.34.73" family: IPv4
}); 
  
// Execute using $ node <filename> 
```

#### 20. ***How does Node.js handle child threads?***
Node.js is a single threaded language which in background uses multiple threads to execute asynchronous code.
Node.js is non-blocking which means that all functions ( callbacks ) are delegated to the event loop and they are ( or can be ) executed by different threads. That is handled by Node.js run-time.

* Nodejs Primary application runs in an event loop, which is in a single thread.
* Background I/O is running in a thread pool that is only accessible to C/C++ or other compiled/native modules and mostly transparent to the JS.
* Node v11/12 now has experimental worker_threads, which is another option.
* Node.js does support forking multiple processes ( which are executed on different cores ).
* It is important to know that state is not shared between master and forked process.
* We can pass messages to forked process ( which is different script ) and to master process from forked process with function send.

