## Concurrency
**Synchronous Programming**
	Program executes top-down
	Guaranteed that previous instructions fully complete before executing the next ones
	very simple to reason about

**Asynchronous Programming**
	Programming has multiple flows of control - called 'threads'
	threads can interleave or run at the same time
![[Pasted image 20241014185021.png]]

*Synchronous* - Process A executes first waits for process B then continues
*Asynchronous* - Process executes first, ask process b to do its work, whilst process b is executing, process a also continues, at some point in the future, process A gets process B's results

### Concurrency models
Two primary models:
*Pre-emptive multitasking*
	code runs in parallel on different CPU cores
	good for cpu-intensive workloads

*Co-operative multitasking*
	tasks that need to be done are queued up and executed in a loop
	good for io-intensive workload

js model is co-operative multitasking.

### Javascript's event loop
![[Pasted image 20241014185632.png]]
each js engine generally follows this procedures
	1. execute js script top-down, registering any handlers
	2. wait for events in  a loop
	3. if an event comes and there is a handler for it, execute the handler to completion
	4. repeat (2) ad infinitum
Browsers handle, counting down items, networking ,adding to the event queue.

#### Event loop consideration
the event loop runs on a single thread - long-running event handlers block everything else, makes the page unresponsive
to avoid blocking:
	make sure all networking is done asynchronously
	cpu-intensive calculation should be pushed to a backend server to process

## Promises
![[Pasted image 20241014190309.png]]
proxy for a future value, evaluated asynchronously, support chaining, branching, error handling 
can be one of the 4 states:
	pending - not evaluated yet
	fulfilled - successfully evaluated
	rejected - failed to evaluate
	settled - either rejected or fulfilled

### Basic API usage
constructor:
	accepts a callback that takes `resolve()` and `reject()` functions
	fulfillment = calling `resolve()`
	rejection = calling `reject()`
.then:
	most common way to chain promises
	executes the next action if the previous one fulfilled
.catch:
	catch-all error handler for the chain above
```javascript
// creates a brand new promise
const myPromise = new Promise((resolve, reject) => {
	// if the action succeeds, call resolve() with the result
	// or, if the action failed, call reject() with the reason
});

myPromise.then (
	() => {
		// this callback will be called if myPromise is fulfilled
	},
	() => {
		// this specific callback will be called if myPromise is rejected
	}
);

// in additon to giving callback for errors in .then(), youi can give a catch-all error handler // as .catch()
myPromsie.catch() (
	() => {
		// handle new problem here
	}
);
```


### Chaining
nested execution of dependent operations
each `.then()` ruins iff the previous promise fulfilled
any error/rejection that happens passed to the next-nearest `.catch()`
after a `.catch()`, more operations can occur
every `.then()` returns a new promise which wraps the previous one
	even if the given callback doesn't return a promise
	stored as russian dolls
	but executed as stack i.e. most-nested happens first

```javascript
const dinnerPlans = startToPrepareDinner()
	.then(lightBBQ)
	.then(stokeFire)
	.then(EAT)
	.catch(eatNothing) 

const restPlans = dinnerPlans
	.then(watchYoutube)
	.then(eveningStroll)
	.catch(goToBed) // maybe the internet was out
```

### Branching
multiple `.then()`'s on the same promise = branching
when the parent promise is resolved, all `.then()` invoked in order
allows for complex control-flow on fulfillment

### Error-handling
errors/exceptions always cause rejections
explicit rejections done via calling `reject()`
any exceptions cause an implicit rejection
`.catch()` clauses can handle errors or pass them to the next `.catch()` by rethrowing
`.finally()` is also available that will run regardless of if an error occurred or not

### Promise orchestration
```javascript
Promise.all() // returns a promise that resolves iff all of the promises passed to it resolve
Promise.allSettled() // returns a promise that resolves once all of the promises passed to it are resolved
Promise.any() // returns a promise that resolves if at least one of the promises passed to it resolves
Promise.reject() // immediately returns a rejected promise with a value
Promise.resolve() // immediately return a resolved promise with a value
```

### The Fetch API
promise-based native js API to download remote resources
resolves if a responsive is received, even if the HTTP status code is not 200
rejects if there are any network error
access the result of the request via changing `then()`'s
```javascript
fetch("https://example.com/moves.json", {
	method: "POST",
});
	// return the body as JSON
	.then(res => res.json())
	// access the JSON
	.then(js => console.log(js));
```

#### Fetch limitations
only works on browsers with Promise support
Not easily cancellable
more complex functionality implemented via Streams API which has non-trivial learning curve


## Async/Await
