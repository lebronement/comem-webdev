# Node.js Introduction

<!-- START doctoc generated TOC please keep comment here to allow auto update -->
<!-- DON'T EDIT THIS SECTION, INSTEAD RE-RUN doctoc TO UPDATE -->


- [What is Node.js?](#what-is-nodejs)
- [Asynchronous](#asynchronous)
- [A synchronous example](#a-synchronous-example)
- [An asynchronous example](#an-asynchronous-example)
- [Node.js callback convention](#nodejs-callback-convention)
- [List of Node.js modules](#list-of-nodejs-modules)
- [Another example (http)](#another-example-http)
- [HTTP module events](#http-module-events)
- [Node.js event loop](#nodejs-event-loop)
- [Node.js event loop](#nodejs-event-loop-1)
- [Resources](#resources)

<!-- END doctoc generated TOC please keep comment here to allow auto update -->

## What is Node.js?

Node.js is a platform built on Chrome's JavaScript runtime for easily building fast, scalable network applications.

Node.js uses an event-driven, non-blocking I/O model that makes it lightweight and efficient, perfect for data-intensive real-time applications that run across distributed devices.

Job trends?

## Asynchronous

Single-threaded & asynchronous

“I am not leaving until you are done...”
vs
“Wake me up when you have result”

## A synchronous example

```js
var fs = require("fs");

/**
 * Simple function to test the synchronous readFileSync function of Node.js
 * @param {string} filename - the file to read
 */
function testSyncRead(filename) {
  console.log("I am about to read a file: " + filename);
  var data = fs.readFileSync(filename);
  console.log("I have read " + data.length + " bytes (synchronously).");
  console.log("I am done.");
}

// We get the file name from the argument passed on the command line
var filename = process.argv[2];

console.log("\nTesting the synchronous call");
testSyncRead(filename);
```

```bash
$ node sample2.js medium.txt

Testing the synchronous call
I am about to read a file: medium.txt
I have read 1024 bytes (synchronously).
I am done.
```

* We use a standard Node module for accessing the file system
* fs.readFileSync is synchronous: it blocks the main thread until the data is available.
* Synchronous functions are easier to use, but they have severe performance implications!!

## An asynchronous example

```js
var fs = require("fs");

/**
 * Simple function to test the asynchronous readFile function of Node.js
 * @param {string} filename - the file to read
 */
function testAsyncRead(filename) {
  console.log("I am about to read a file: " + filename);

  fs.readFile(filename, function (err, data) {
    console.log("Nodes just told me that I have read the file.”);
  });

  console.log("I am done. Am I?");
}
// We get the file name from the argument passed on the command line
var filename = process.argv[2];

console.log("\nTesting the asynchronous call");
testAsyncRead(filename);
```

```js
$ node sample2.js medium.txt

Testing the asynchronous call
I am about to read a file: medium.txt
I am done. Am I?
Nodes just told me that I have read the file.
```

* fs.readFile is asynchronous: it does not block the main thread until the data is available.
* We must provide a callback function, which Node.js will invoke when the data is available.
* Problems can happen when an (asynchronous) function is called.
* Node.js developers have to learn the asynchronous programming style.

## Node.js callback convention

When calling an asynchronous operation, the convention in Node.js is to use a callback function that will be called with 2 arguments. If an error occurs, the first argument will be an error describing the problem. If the operation succeeds, the second argument will be the result. You will only receive one or the other, not both.

```js
var fs = require("fs");

fs.readFile(filename, function (err, data) {

  if (err) {
    // handle the error
    console.warn("An error occurred: " + err.message);
    return;
  }

  // do something with the data
  process(data)
});
```

* The callback function with its two arguments.
* Before doing anything with the data, check if there is an error.
* If there was an error, handle it and stop there. There is no data available.
* Otherwise, if there was no error, you may use the data.

## List of Node.js modules

...

## Another example (http)

```js
/*global require */

var http = require("http");

/**
 * This function starts a http daemon on port 9000. It also
 * registers a callback handler, to handle incoming HTTP
 * requests (a simple message is sent back to clients).
 */
function runHttpServer() {
  var server = http.createServer();

  server.on("request", function (req, res) {
    console.log("A request has arrived: URL=" + req.url);
    res.writeHead(200, {
      'Content-Type': 'text/plain'
    });
    res.end('Hello World\n');
  });

  console.log("Starting http server...");
  server.listen(9000);
}

runHttpServer();
```

* We use a standard Node module that takes care of the HTTP protocol.
* Node can provide us with a ready-to-use server.
* We can attach event handlers to the server. Node will notify us asynchronously, and give us access to the request and response.
* We can send back data to the client.
* We have wired everything, let’s welcome clients!

## HTTP module events

Screenshot of http docs

* These are events that are emitted by the class. You can write callbacks and react to these events.

## Node.js event loop

Callback functions that you have written and registered

```js
on(’request’, function(req, res) { // my code});
on(’data’, function(data) { // my code});
```

* Event loop: get the next event in the queue; invoke the registered callbacks in sequence; delegate I/O operations to the Node platform

* Queue of events that have been emitted
* All the code that you write runs on a single thread
* The long-running tasks (I/Os) are executed by Node in parallel; Node emits events to report progress (which triggers your callbacks).
* Another pattern is to provide a callback to node when invoking an asynchronous function.

## Node.js event loop

<img src='images/node-event-loop.png' width='100%' />

## Resources

* Understanding the Node.js Event Loop
  http://strongloop.com/strongblog/node-js-event-loop/
* Mixu's Node book: What is Node.js? (chapter 2)
  http://book.mixu.net/node/ch2.html
* Node.js Explained, video
  http://kunkle.org/talks/