# Reactive forms -Approach

## Set up

- Forms are created programmtically

- set a property in app component which holds the form.

- property is of type FormGroup imported angular/forms

- for reactive approach to work, ie, when v connect the progarmmatically created form wwith html code - v have to import ReactiveForms module in app.module.ts

- No need of FormsModule since it used for template driven approach.

**app.component.ts**

```typescript
signupForm: FormGroup;
```

**app.module.ts**

```typescript
import { ReactiveFormsModule } from '@angular/forms';
imports: [BrowserModule, ReactiveFormsModule];
```

---

## Creating a basic form in app component

- Initialize form in ngOnInIt()

- set property signupForm with new FormGroup().

- FormGroup() takes a js object

- this object have controls for our form ehich is of {key:value} pairs

- key will be each inputs - value is new FormControl()

- FormControl() - 3 args - initial formstate and 2 validators.

**app.component,ts**

```typscript
  signupForm: FormGroup;

  // initialize a form in ngOnInIt
  ngOnInit() {
    this.signupForm = new FormGroup({
      username: new FormControl(null),
      email: new FormControl(null),
      gender: new FormControl(this.genders[0]),
    });
  }
```

- thus we initialize a basic form.

---

## Sync html form and typescript form

- add a directive _formGroup_ to form element in html

- pass _signupForm_ property as a property binding to the _formGroup_ directive

- thus v sync both html form and form initialized in app.component.ts

- next v have to sync controls specified in the typescript form with the correct inputs in the html form. ie what controls connect to what inputs.

- for this v add _formControlName_ directive to each inputs in html form and pass the control related to that directive.

- thus v sync controls in typescript form with inputs in html form.

**app.component.ts**

```typsecript
  ngOnInit() {
    this.signupForm = new FormGroup({
      username: new FormControl(null),
    });
  }
```

**app.component.html**

```html
<form [formGroup]="signupForm">
  <div class="form-group">
    <label for="username">Username</label>
    <input
      formControlName="username"
      type="text"
      id="username"
      class="form-control"
      name="username"
    />
  </div>
</form>
```

---

## Submitting the form

- add ngSubmit directive and assign it with method

- this methid is execute while form is submitted.

**app.component.html**

```html
<form [formGroup]="signupForm" (ngSubmit)="onSubmit()"></form>
```

**app.component.ts**

```typescript
onSubmit(){

// access the form.
console.log(this.signupForm);
}

```

- here in this apprach no need to use local reference for form access in app.component.ts, since here form is initiazlied in app.component.ts itself.

---

## Adding Validation

- Unlike template driven approach where v add the validations in template, here v add validators as an argument of FormControl. here v r configuring validators in typescript code not in template.

**app.component.ts**

```typescript
ngOnInit() {
this.signupForm = new FormGroup({
username: new FormControl(null, Validators.required),
email: new FormControl(null, [Validators.required, Validators.email]),
gender: new FormControl(this.genders[0]),
});
}

```

- add Validators.required as argument here

- can also give the validators in array.

---

## Accessing the controls from html template.

- when v want to show some error message when input v entered is invalid/not in correct format.

**app.component.html**

```html
<div class="form-group">
  <label for="username">Username</label>
  <input
    formControlName="username"
    type="text"
    id="username"
    class="form-control"
    name="username"
  />
  <small
    class="text-primary"
    *ngIf="
      signupForm.get('username').invalid &&
      signupForm.get('username').touched
    "
    >please add a username</small
  >
</div>
```

- get() method allows us to access he controls easily.
- here v first access form - call get() - pass controls as string - check whether is invalid/not.
- here, form is _signupForm_
- here, controls is _username_

- here this invalid, touched etc .. are properties of _FormGroup_ object. in case of _template driven_ it is a property of _NgForm_

- can also use this to style form when it is not valid.

**app.component.css**

```css
input.ng-invalid.ng-touched {
  border: 1px solid red;
}
```

---

## Grouping controls

- not working

-Check the documenatation.

![Grouping control](https://angular.io/guide/reactive-forms#grouping-form-controls)

---

## Arrays of Form Controls

- Adding controls to an array

- add a control called _hobbies_ in to FormGroup which is of type _FormArray_.

- add a method in app compoenent, where v push the controls to the hobbies.

- so when v click on a button, v add an input control to hobbies array.

- next we have to sync this array in app component with html code.

- Add a directive called _formArrayName_ to that elements div.

- assign hobbies control to it.

- next loop thru that array - also get the index of each.

- later add formControlName directive to the input - assign index i as a property binding to that directive.

**app.component.ts**

```typescript
ngOnInit() {
this.signupForm = new FormGroup({
username: new FormControl("", Validators.required),
email: new FormControl("", [Validators.required, Validators.email]),
phone: new FormControl("", Validators.required),

gender: new FormControl(this.genders[0]),
hobbies: new FormArray([]),
});
}

```

**app.component.html**

```html
<div formArrayName="hobbies">
  <h1 class="display-5">Hobbies</h1>
  <button class="btn btn-default" type="button" (click)="onAddHobby()">
    Add Hobby
  </button>

  <div
    class="form-group"
    *ngFor="let hobbyControl of signupForm.get('hobbies')['controls']; let i = index"
  >
    <input
      type="text"
      name="hobby"
      id="hobby"
      [formControlName]="i"
      class="form-control"
    />
  </div>
</div>
```

- here v do the property bindingon formControlName directive since v r assigning an index to it, not a control.

---

## reactive forms - custom validators

- ie adding our own validators-
- for instance v want to forbid users from using some usernames. in this case, we add a property called _forbiddennames_

```typescript
forbiddennames = ['chris', 'annah'];
```

- next add a validator function named _forbiddenNames_
- validator gets an argument of type _FormControl_.
- also it should return a js object.
- where key is a string, value is a boolean.

```typescript
forbiddenNames(control: FormControl): any{}
```

- next check whether any item in _forbiddenNames_ array are present in argument received. it returns -1 if value not present. -1 interpret as _true_. so we have to check this is not equal to -1. returns valid if value not present, else invalid.
- if names matches, return an object {},
- else return null

```typescript
if (this.forbiddenName.indexOf(control.value) !== -1) {
  return { forbiddenName: true };
} else {
  return null;
}
```

- next pass forbiddenNames custom validator to username form control.

```typescript
ngOnInit() {
    this.signUpForm = new FormGroup({
      username: new FormControl(null, [
        Validators.required,
        this.forbiddenNames,
      ]),
      email: new FormControl(null, [Validators.required, Validators.email]),
      phone: new FormControl(null, Validators.required),
      gender: new FormControl(this.genders[0]),
      hobbies: new FormArray([]),
    });
  }
```

- next after it raises error,

  > core.js:4197 ERROR TypeError: Cannot read property 'forbiddenName' of undefined

- to solve it attach **bind()** to **this.forbiddenNames** which present in ngOnInit. and it is called by angular when doing validation. at that time, _this_ not refer to our class. to work this, attach _bind(this)_ to **this.forbiddenNames**.

```typescript
username: new FormControl(null, [
  Validators.required,
  this.forbiddenNames.bind(this),
]);
```

---

## using error codes.

```html
<small
  class="text-danger"
  *ngIf="
              signUpForm.get('username').invalid &&
              signUpForm.get('username').touched
            "
>
  <small
    class="text-danger"
    *ngIf="signUpForm.get('username').errors['nameIsForbidden']"
  >
    The username is not invalid
  </small>
  <small
    class="text-danger"
    *ngIf="signUpForm.get('username').errors['required']"
  >
    username is required
  </small>
</small>
```

## creating a custom async validator

- using asynchronous validators

### creating asynchronous validator

```typescript
// asynchronous method
forbiddenLocation(valueRec: FormControl): any {
  const promise = new Promise<any>((resolve, reject) => {
    setTimeout(() => {
      if (valueRec.value === 'kochi') {
        resolve({ locationIsForbidden: true });
      } else {
        resolve(null);
      }
    }, 1500);
  });
  // finally return the promise
  return promise;
}
```

- next add asynchronous validator on ngOnIt() method.

```typescript
location: new FormControl(null, [Validators.required], this.forbiddenLocation);
```

---

## reacting to status and value changes

- using valueChanges and statusChanges observables.
  - valueChanges fired whenever v make changes in form.
  - statusChanges fires whenever form status changes(INVALID, PENDING, VALID).

**app.component.ts**

```typescript
// using valueChanges observable
this.signUpForm.valueChanges.subscribe((value) => {
  console.log(value);
});

// using statusChange observable
this.signUpForm.statusChanges.subscribe((value) => {
  console.log(value);
});
```

---

## Setting and patching values

- use setValue() for pre-populating the field when page loads.
- use patchValue() to update a part of the form.
- reset form after submit.

**app.component.ts**

```typescript
ngOnInit() {
  // setValue
  this.signUpForm.setValue({
    username: 'max',
    email: 'jose@gmail.com',
    phone: 2342342342,
    location: 'london',
    gender: 'male',
    hobbies: [],
  });

  // patchValue()
  this.signUpForm.patchValue({
    username: 'lee',
    email: 'lee@gmail.com',
  });
}
// form submit - form reset after
  onSubmit(): void {
    console.log(this.signUpForm);
    this.signUpForm.reset();
  }
```

---
