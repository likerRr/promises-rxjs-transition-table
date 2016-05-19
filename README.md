# Promises -> RxJs
### Transition table
A set of techniques used with promises and their analogues in rxjs

## Resolve a value
Sometimes you just need a way to resolve a simple value but wrapped by promise
> Promise
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

> Promise
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
