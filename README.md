# Promises -> RxJs
### Transition table
A set of techniques used with promises and their analogues in rxjs

## Resolve a value
Sometimes you just need a way to resolve a simple value but wrapped by promise
> ES6 Promise
```javascript
Promise.resolve(2)
  .then(val => console.log(val));
// -> 2
```

> RxJs
```javascript
Rx.Observable.of(3)
  .subscribe(val => console.log(val));
// -> 3
```

Refs:
* [Live example](https://jsbin.com/racidab/edit?js,console)
* [.of()](http://reactivex.io/rxjs/class/es6/Observable.js~Observable.html#static-method-of)
* [.subscribe()](http://reactivex.io/rxjs/class/es6/Observable.js~Observable.html#instance-method-subscribe)

## Reject a value or throw an exception

> ES6 Promise
```javascript
Promise.reject('Invalid value')
  .catch(err => console.log(err));
// -> Invalid value    
```

> RxJs
```javascript
Rx.Observable.throw('Invalid value')
  .subscribe(() => {}, err => console.log(val));
// -> Invalid value    
```

Refs:
* [Live example](https://jsbin.com/bunuxu/edit?js,console)
* [.throw()](http://reactivex.io/rxjs/class/es6/Observable.js~Observable.html#static-method-throw)
* [.subscribe()](http://reactivex.io/rxjs/class/es6/Observable.js~Observable.html#instance-method-subscribe)

## Finalize - callback that calls either of success or fail result of task

> [Q](https://github.com/kriskowal/q) Promise
```javascript
Q.reject(1)
  .finally(() => console.log('Finished'));
// -> "Finished" 
```

> RxJs
```javascript
// * finally should be called before subscribe
// * error callback of observer should be defined, otherwise finally will not be called
Rx.Observable.throw(1)
  .finally(() => console.log('Finished'))
  .subscribe(null, () => {});
// -> "Finished"
>
// * when error callback isn't defined
Rx.Observable.throw(1)
  .finally(() => console.log('Finished'))
  .subscribe();
// -> "Uncaught error"
```

Refs:
* [Live example](https://jsbin.com/bazoxo/edit?js,console)
* [.finally()](https://github.com/Reactive-Extensions/RxJS/blob/master/doc/api/core/operators/finally.md)
* [.subscribe()](http://reactivex.io/rxjs/class/es6/Observable.js~Observable.html#instance-method-subscribe)

## Async sequence of tasks
You have an array of tasks that should be done in parallel and you want to be notified when that process ends

> Tasks
```javascript
function asyncTask1() {
  return 1;
}
function asyncTask2() {
  return 2;
}
function asyncTask3() {
  return 3;
}
```

> ES6 Promise
```javascript
Promise.all([asyncTask1(), asyncTask2(), asyncTask3()])
  .then(res => console.log(res));
// -> [1, 2, 3]
```

> RxJs #1
```javascript
// * in case when you want to be notified about completion of each task in the sequence only when they are all finished
// * subscribe fires only once when the sequence completed
Rx.Observable.forkJoin(Rx.Observable.of(1), Rx.Observable.of(2), Rx.Observable.of(3))
  .subscribe(res => console.log(res));
// -> [1, 2, 3]
```

> RxJs #2
```javascript
// * in case when you want to be notified about completion of each task in the sequence as soon as it completes
// * subscribe fires three times when each of the tasks completed
Rx.Observable.from([asyncTask1(), asyncTask2(), asyncTask3()])
  .subscribe(res => console.log(res));
// -> 1
// -> 2
// -> 3
```

Refs:
* [Live example](https://jsbin.com/dawawe/edit?js,console)
* [.forkJoin()](https://github.com/Reactive-Extensions/RxJS/blob/master/doc/api/core/operators/forkjoin.md)
* [.of()](http://reactivex.io/rxjs/class/es6/Observable.js~Observable.html#static-method-of)
* .from() [1](http://reactivex.io/rxjs/class/es6/Observable.js~Observable.html#static-method-from), [2](https://github.com/Reactive-Extensions/RxJS/blob/master/doc/api/core/operators/from.md)
* [.subscribe()](http://reactivex.io/rxjs/class/es6/Observable.js~Observable.html#instance-method-subscribe)

## Creating an instance of promise/observable
Sometimes you need to implement your own logic of deferred task

> ES6 Promise
```javascript
function newPromise(val) {
  return new Promise((resolve, reject) => {
    if (val === 1) {
      resolve(val);
    } else {
      reject(false);
    }
  });
}
>
newPromise(1)
  .then(res => console.log(res))
  .catch(err => console.log(err));
// -> 1
```

> RxJs
```javascript
function newObservable(val) {
  return Rx.Observable.create(observer => {
    if (val === 1) {
      observer.next(val);
      observer.complete();
    } else {
      observer.error(false);
    }
  });
}
>
newObservable(1)
  .subscribe(
    res => console.log(res), 
    err => console.log(err), 
    () => console.log('complete')
  );
// -> 1
// -> 'complete'
```

## Sync sequence of tasks
// In progress