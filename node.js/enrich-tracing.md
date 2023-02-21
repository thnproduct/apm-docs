# Enrich Tracing in Node.js

### Manual Instrumentation with Open Tracing

Thundra's Node.js agent automatically traces a broad range of libraries by default when integration is complete. However, if you would like to fine-tune your traces and perform manual instrumentation, then you will need to get a tracer instance with the command `thundra.tracer()` in your Lambda function.

#### Using Manual Instrumentation

In your Lambda Handler code, add instrumentation to the operations you want to track. You can do that primarily by placing "spans" around operations of interest and adding log statements to capture useful data relevant to those operations. Below is a simple hello-world example:&#x20;

{% code title="Manual instrumentation example" %}
```javascript
const thundra = require("@thundra/core"); 

exports.handler = thundra()((event, context,callback) => {
  // Get the tracer instance
  const tracer = thundra.tracer();
  // Manually create a new span to represent some group of operations
  const parent = tracer.startSpan('parentSpan');
  console.log('Doing some work in the parent span...');
  // Create another span under the above span
  const child = tracer.startSpan('childSpan');
  console.log('In the child span...');
  // Finish the innermost span(childSpan)
  // You can use OpenTracing compatible span.close() method
  child.close();
  // Finish the parentSpan 
  // This time using the tracer.finishSpan() method provided by Thundra
  tracer.finishSpan();
  callback(null, 'Success');
});   
```
{% endcode %}

#### Thundra Specific Methods

Apart from being OpenTracing compatible, Thundra's Node.js agent also provides specific methods to allow easier configuration of Trace Support.

**getActiveSpan/finishSpan**

The Thundra tracer provides additional methods beyond those found in the OpenTracing spec.

Below you can see how to obtain an active span via the `getActiveSpan` method and finish the active span via the `finishSpan` method.



{% code title="getActiveSpan() Example" %}
```javascript
// In your handler

const tracer = thundra.tracer();

const parent = tracer.startSpan('parent');
const child = tracer.startSpan('child', {childOf: parent});

const span = tracer.getActiveSpan(); //return child span as current active span

tracer.finishSpan(); // closes children and active span is parent
tracer.finishSpan(); // closes parent and active span is null
```
{% endcode %}

**Callback Wrapper**

The Thundra tracer provides a wrapper that will wrap your asynchronous callback. The wrapper returns a closure that saves the current active span at the time of callback creation, and then uses the saved active span as a parent span when the callback occurs. In the example below, the Thundra tracer’s `wrapper` method wraps a callback function with a try/catch statement. It starts a span with the name `callback` at the beginning and closes the span either before returning or in the case of an error.

{% code title="Callback Wrapper Example" %}
```javascript
// In your handler

const tracer = thundra.tracer();

const callback = function() {
  console.log('callback called');
};

const parentSpan = tracer.startSpan('parent');

// Callback is wrapped with span whose parent will be parentSpan which is the current active span 
const wrappedCallback = tracer.wrapper('callback', callback); 

const doStuff = function(stuff, callback) {
  const doStuffSpan = tracer.startSpan(‘doStuff’);
  console.log(`Starting doing my ${stuff}`);
  callback();
  doStuffSpan.finish();
}

doStuff('yoga', wrappedCallback);

parentSpan.finish(); 

// Span tree right now :
//         parentSpan
//            /  \
//      callback doStuff


// Span tree without wrapper would be:
//         parentSpan
//            /  
//      doStuff
//         /
//     callback
```
{% endcode %}

