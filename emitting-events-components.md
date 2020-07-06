# emitting events across components

- Suppose user component has a button when v click that, we have to show some contents in app component

1. create a service.ts file in user component

```bash
ng g s user/ --skipTests=true
```

2. add an event emitter in user.service which returns a boolean.

```typescript
  activateEmitter = new EventEmitter<boolean>();
```

3. Inject this service on user component.

4. add new method - invoke userservice injected- call emitter - emit a true value.

```typescript
  
  // inject user srvice to user component.
  constructor(
    private route: ActivatedRoute,
    private userService: UserService
  ) {}
  
  // add new method
  onActivate() {
    // invoke userservice - call emitter - emit true
    this.userService.activateEmitter.emit(true);
  }

```

5. In user component view, add a click listener to button

```html
<button class="btn btn-primary" (click)="onActivate()">Activated</button>

```


# Next Listen to Events Emitted from app component.


1. inject userService to app component

2. initiliaze a property to false

3. In ngOnInit(), access userSerice inject- call emitter - subscribe to emited data - set data to property.

```typescript
 ngOnInit() {
    // access userSerice inject- call emitter - subscribe to emited data - set data to property.
    this.userService.activateEmitter.subscribe((data: boolean) => {
      this.userActivated = data;
    });
  }
```

4. set ngIf to userActivated property in app component view.

```html
  <p *ngIf="userActivated">Activated</p>

```
- Here, when ngIf is true, content in p tag is shown else not.
 


## Use Subject Instead of Event Emitter

1. import subject from rxjs - define an object of Subject

```typescript
import { Subject } from "rxjs";
activateEmitter = new Subject<boolean>();
```
2. call next() on emitter in instead of emit() in onActivate()

```typescript
// add new method
onActivate() {
// invoke userservice - call emitter - call next() since v user Subject
this.userService.activateEmitter.next(true);
}
```

- Subject is similar to Observable.
- call next(),
- Use Subject rather than EventEmitter.
- since v use subject we have to call unsubscribe() 

3. define subscription object
4. store subscrition to object
5. in ngOnDestroy(), call unsubscribe on subscription object.

```typescript
import { Subscription } from "rxjs";

  private activatedSub: Subscription;

  ngOnInit() {
    // access userSerice inject- call emitter - subscribe to emited data - set data to property - store subscription object.
    this.activatedSub = this.userService.activateEmitter.subscribe(
      (data: boolean) => {
        this.userActivated = data;
      }
    );
  }
  // unsubscribe subjects like observables, when not needed,
  ngOnDestroy() {
    this.activatedSub.unsubscribe();
  }
```

Note: Only Use Subject when we want to communcate events across the components through services.














