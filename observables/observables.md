
```sh
let numbers = [1,5,10];
//create Observable
let source = Observable.from(numbers);

//implement Observer, takes 3 functions next,error and complete
class myObserver {
	next(value){
		console.log(value);
	}
	error(e){
		console.log("error occurred!")
	}
	complete(){
		console.log("completed!")
	}
}
//subscribe the observer
source.subscribe(new myObserver());
```

The above can be simplified by moving the implementation into subscribe and using the arrow functions like this
```sh
source.subscribe(
		value => console.log(value),
		error => console.log("error occurred!"),
		() => console.log("completed!")
);
```

 #### Other way to create an Observable stream

```sh
let numbers = [1,5,10];
let source = Observable.create(observer => 
        for(let n of numbers){
			observer.next(n);
		}
		observer.complete();
)
// This is low level implementation of Observable.from
```

## Observables Temparature (HOT & COLD)
---
 A Hot Observable is an actively producing sequence irrespective of its subscriptions
 
 Example: A stock ticker - constantly updates the price depending on the stock movement, irrespective of whether anyone is watching it or not.
 
 A cold Observable sequences are passive; Meaning they start producing only on subscription 
 
Example:
```ts
let coldObservable = Observable.interval(1000);
coldObservable.subscribe(x => console.log("A: ${x}"));
//sleep 2000
coldObservable.subscribe(x => console.log("B: ${x}"));

output: 
A:0 <-- A subscribed here; starts producing the sequence
A:1
A:2
B:0 <-- B subscribed here; It starts from 0, even though the latest value the last subscriber got is 2
B:1
A:3
B:2
.
.
.
```
At any given point in time:
 - if n subscribers always get same value from the Observable, such Observable is termed as Hot Observable.
 - if n subscribers get different values from the Observable, such Observable is termed as Cold Observable.
 
##### Cold Observable #####
---
```sh
let cold = Observable.create(observer =>
        observer.next(Math.random())
)
cold.subscribe(n => console.log("Subscriber A : ${n}")
cold.subscribe(n => console.log("Subscriber B : ${n}")
// Here each of the subscribers gets different random number.
```

> how to turn a cold observable to hot observable?
  - Move the Math.random() outside of the create method.
 ##### Hot Observable  #####
 ---
```sh
const x = Math.random();
let hot = Observable.create(observer =>
        observer.next(x)
)
hot.subscribe(n => console.log("Subscriber A : ${n}")
hot.subscribe(n => console.log("Subscriber B : ${n}")
// Irrespective of the number of subscribers, each of the subscribers gets the same random number.
```

Now, without touching the Observable.create implementaion, we can still convert a cold observable to hot observable by means of ***publish*** and ***connect*** methods
```ts

let cold = Observable.create(observer =>
        observer.next(Math.random())
).publish();

cold.subscribe(n => console.log("Subscriber A : ${n}")
cold.subscribe(n => console.log("Subscriber B : ${n}")

cold.connect()
// once you connect, publish lets you to publish the latest value to all its subscribers.

--------------------------------------------------------------------

var publishedColdObservable = Observable.interval(1000).publish();
publishedColdObservable.subscribe(x => console.log("A: ${x}"));
// sleep 2000
publishedColdObservable.subscribe(x => console.log("B: ${x}"));

// Connect the source
var connection = publishedColdObservable.connect();

output:
A:0
B:0
A:1
B:1
A:2
B:2
.
.
.
```