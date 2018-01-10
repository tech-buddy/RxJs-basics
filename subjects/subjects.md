# Subject

 > What is Subject?

 - Subject -> is both Observable and Observer
![subject](https://gist.githubusercontent.com/lava-envista/06712e40b8479e845a18125726f5960b/raw/93585e37dee29505ee05236c853fabdba2d390c8/subject.png)
 - It extends an Observable class and also implements a Subscription Interface.
 That means, you get the behavior of an Observable as well as an Observer.
 
 ```sh

   Observable          Observer/Observable
 |------------|          |---------|---------------------> subject observer 1 
 | Main Source| -------> | Subject |---------------------> subject observer 2
 |------------|          |---------|---------------------> subject observer 3        
     				   can push more values here 		
```

 - So, You can use a subject to subscribe all the observers, and then subscribe the subject to a backend data source. Here it is basically acting as a proxy between the backend data source and all the observers subscribed to this subject.

 ***The easiest way to understand a Subject is to see it as a pushable observable.***

### using subjects
In the following example, let us see how subject is both proxy and a broadcaster.
```ts
let source = Observable.interval(1000);
let Subject subject = new Subject();
source.subscribe(subject); // now it can act as a proxy

let subSubject1 = subject.subscribe(x => console.log("1: subscribed to subject");
let subSubject2 = subject.subscribe(y => console.log("2: subscribed to subject");
//broadcasting to subscribers.
```
Here We first create a source sequence which produces an integer every 1 second. We then create a Subject, and pass it as an observer to the source so that it will receive all the values pushed out by this source sequence. After that, we create another two subscriptions, this time with the subject as the source. The subSubject1 and subSubject2 subscriptions will then receive any value passed down (from the source) by the Subject.  


Subject is the basic implementation of ISubject Interface from Rx Library. There are other implementations that offer different functionalities. Basically all of these types store some (or all of) values pushed to them via next , and broadcast it back to its observers. In this way, they convert a Hot Observable into a Cold one. 

> ReplaySubject - Stores all the values that it has published. So, when you subscribe to it, you receive entire history of values that are published, even though your subscription might have happened after certain values are pushed out.

> BehaviorSubject - Same as ReplaySubject, except that it stores only the last value. When you subscribe to it, you receive the latest value that is pushed out. Additionally it needs a initial value upon initialization.

- This is useful in cases where you will need to know the current state, immediately after you subscribe to it.

> AsyncSubject - Stores only the last value and publishes it when the sequence it complete. AsyncSubjects can be used when the source Observable is hot and might complete before any observer can subscribe to it. Here AsyncSubject can still cater to future subscriptions even though the source Observable is complete.