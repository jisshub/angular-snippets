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
import { ReactiveFormsModule } from "@angular/forms";
imports: [BrowserModule, ReactiveFormsModule]
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

- add a directive *formGroup* to form element in html

- pass *signupForm* property as a property binding to the *formGroup* directive

- thus v sync both html form and form initialized in app.component.ts

- next v have to sync controls specified in the typescript form with the correct inputs in the html form. ie what controls connect to what inputs.

- for this v add *formControlName* directive to each inputs in html form and pass the control related to that directive.

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
  <input formControlName="username" type="text" id="username" class="form-control" name="username">
</div>

```	
---

##  










