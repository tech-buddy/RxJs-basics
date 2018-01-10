
## RX Operators
---
### Rx Operators are used to perform some operations on the asynchronous data stream

> Let say you want to double the value of each of the observable, use map operator like this
```ts
let numbers = [1,5,10];
//create Observable
let source = Observable.from(numbers).map(n => n*2);
```
> Let say you want to double the value of each of the observable, and filter out values > 4 use map & filter operator together like this
```ts
let numbers = [1,5,10];
//create Observable
let source = Observable.from(numbers).map(n => n*2).filter(n => n > 4);
```

> We can also generate Observables from the DOM like this
```ts
let clicks = Observable.fromEvent(document, 'click');
// For every click event on the document, an observable is created; further we can subscribe to those Observables like this
clicks.subscribe(click => console.log(click));
```

> Emitting observable every second like so
```sh
let interval = Observable.interval(1000);
interval.subscribe(second => console.log(new Date().getSeconds()));
```

> Emitting Observable after certain time limit like so
```sh
let timer = Observable.timer(2000);
timer.subscribe(value => console.log("i'm printing after 2 seconds"));
```



