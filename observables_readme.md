# observables

- observable - data source - like user inputs - http requests.

- imported frm 3rd part y pacjkage called rxjs

- observer - handles data

```typescript
import { Component, OnInit, OnDestroy } from "@angular/core";
import { interval, Subscription } from "rxjs";
@Component({
  selector: "app-home",
  templateUrl: "./home.component.html",
  styleUrls: ["./home.component.css"],
})
export class HomeComponent implements OnInit, OnDestroy {
  private firstSubscription: Subscription;
  constructor() {}

  ngOnInit() {
    // store subscription object reqtured frm subcribe() to firstSubscription.
    this.firstSubscription = interval(1000).subscribe((count) => {
      // count - observer which gets the user input.
      console.log(count);
    });
  }
  
  // define ngOnDestory() to unsubscribe() observable
  ngOnDestroy() {
    // call unsubscribe(), here v unsubsribe the observable, ie stops its execution while moving to another component.
    this.firstSubscription.unsubscribe();
  }
}

```
- The subscribe() call returns a Subscription object that has an unsubscribe() method, which you call to stop receiving notifications.

- If we dont the unsubscribe the observable it stlls excuets even v nvigate to another component.

- In above example, if we navigate to other componen from home, observable is stopped and will not run until v navigate back to Home component

## Building a Custom Observable

```typescript
ngOnInit(){
// building a custom observable
    const customObservable = Observable.create((observer) => {
      let count = 0;
      setInterval(() => {
        observer.next(count); // observer emits new value
        count++;
      }, 1000);
    });

    // subscribe to customObservable object - save to subscription object
    this.firstSubscription = customObservable.subscribe((data) => {
      console.log(data);
    });
  }
  ngOnDestroy() {
    // here v unsubsribe the observable, ie stops the execution while moving to another component
    // invoke unsubscribe() on subscription object
    this.firstSubscription.unsubscribe();
  }
```
### steps

1. create an observable 

2. subscribe to that observable

3. store the observable to subscription object

4. define ngOnDestroy()

5. call unsubscribe() on subscription object



## Errors in Observables

- observer.error() -> used to throw errors.

- once an error occurs observables not emit any values later on.


## Completeing the error,

- observer.complete()

- when calling *complete* on observer, observable comes to halt.
- thus doesnt reaches the *error*, noe values emitted therafter
	
- no need to unsubcribe the observable once it is completed.
		
- once *error* is thrown by observer before *complete* then observer.complete() is not called.


**home.component.ts**

```typescript
export class HomeComponent implements OnInit, OnDestroy {
  private firstSubscription: Subscription;
  constructor() {}

ngOnInit() {

    // building a custom observable
    const customObservable = Observable.create((observer) => {
      let count = 0;
      setInterval(() => {
        observer.next(count); // observer emits new value6
        if (count === 5) {
          observer.complete(); //observer halts the observable thereafter, ie no data/error is emitted afterwards
        }
        if (count > 3) {
          observer.error(new Error("count exceeds the limit")); // observer throws error
        }
        count++;
      }, 1000);
    });

    // subscribe to customObservable object - save to subscription object
    this.firstSubscription = customObservable.subscribe(
      // here v subscribe to handle data emitted by custom observable
      (data) => {
        console.log(data);
      },
      // subscribe to handle error thrown
      (error) => {
        console.log(error);
        // alert a message
        alert(error.message);
      },
      // subscribe to handle complete, no nedd of any parameters since no args r passed after complete()
      () => {
        console.log("Completed!");
      }
    );
  }
  ngOnDestroy() {
    // here v unsubsribe the observable, ie stops the execution while moving to another component
    // invoke unsubscribe() on subscription object
    this.firstSubscription.unsubscribe();
  }
}
```	
	
		

