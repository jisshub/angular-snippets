# Forms in Angular

- Forms r ahandled by angular and later sibmitted to the server.
- Angulars job is to give a js object representation of tht form - making ti easy to retrieve the user inputed values - provide the state of the form.

**ScreenShots**
![image](./screenshots/Screenshot from 2020-07-06 12-34-11.png 'image')


---

## Two Approaches in handling the form

1. template Driven Approach

- here user set up the form in html - angular 	automatically infer the structure of the form.

2. Reactive approach(complex)

- here v defien the form structure in typescript code. also set up the html code for form - finally connect both mannualy.
---

# Template Driven approach
## Creating the form - Registering controls

- import the formsModule from angular/forms

- if forms moduleis imported, angular create a js obnject representation of our form if it detects the html code for the form.

- here v need to tell angular what controls are needed to registered in the js representation of the form.

- to do this, v add ngModel directive to the inout element. thud v tell angulsr that inout is a control in the form

- To recognize this input as a control in the form by angular, v have to give name attribute for that input.

- thus, this control is registered in the js representation of form.

**app.component.html**

```html

<div class="container">
  <div class="row">
    <div class="col-xs-12 col-sm-10 col-md-8 col-sm-offset-1 col-md-offset-2">
      <form>
        <div id="user-data">
          <div class="form-group">
            <label for="username">Username</label>
            <input type="text" id="username" class="form-control" ngModel name="username">
          </div>
          <button class="btn btn-default" type="button">Suggest an Username</button>
          <div class="form-group">
            <label for="email">Mail</label>
            <input type="email" id="email" class="form-control" ngModel name="email">
          </div>
        </div>
        <div class="form-group">
          <label for="secret">Secret Questions</label>
          <select id="secret" class="form-control" ngModel name="secret">
            <option value="pet">Your first Pet?</option>
            <option value="teacher">Your first teacher?</option>
          </select>
        </div>
        <button class="btn btn-primary" type="submit">Submit</button>
      </form>
    </div>
  </div>
</div>

```
---

## Submitting the Form 

- we use ngSubmit directive in the form element.
- ngSubmit fires a submit event only when the form is submitted.
- we call a method that assigned to ngSubmit when the event fires.
- define that method in component.ts.


**app.component.html**
```html
<form (ngSubmit)="onSubmit()">
        <div id="user-data">
          <div class="form-group">
            <label for="username">Username</label>
            <input type="text" id="username" class="form-control" ngModel name="username">
          </div>
          <button class="btn btn-default" type="button">Suggest an Username</button>
          <div class="form-group">
            <label for="email">Mail</label>
            <input type="email" id="email" class="form-control" ngModel name="email">
          </div>
        </div>
        <div class="form-group">
          <label for="secret">Secret Questions</label>
          <select id="secret" class="form-control" ngModel name="secret">
            <option value="pet">Your first Pet?</option>
            <option value="teacher">Your first teacher?</option>
          </select>
        </div>
        <button class="btn btn-primary" type="submit">Submit</button>
      </form>

```
**app.component.ts**

```typescript
 // define onSubmit() method
  onSubmit() {
    console.log("Completed!");
  }
```

## Accessing the User Inputed values from form object


- For that purpose, v set a local reference in the form element- assign ngForm directive to the localreference as string - pass that local reference as an argument to the method.

**app.component.html**
```html
<form (ngSubmit)="onSubmit(form1)" #form1="ngForm">
```

- Later, provide a parameter in onSubmit() method definition which is of type *NgForm*.

**app.component.ts**
```typescript
onSubmit(form: NgForm) {
    console.log(form);
  }
```
- once v submit this form v get js object representation of form.

- In that object, select value property which is an object - contains {key: value} pairs - where keys are controls - values r the user entered inputs.

- this is how we get access to the form objects made by angular on submiting the form. 

---

## Acesing the form using @ViewChild


**app.compoenent.ts**
```typescript
// use @viewchid to access the form object - pass localreference as first argument - set type as NgForm

@ViewChild("form1", { static: true }) signupForm: NgForm;

onSubmit(){
	// log the form object
 console.log(this.signupForm)
}
```

## Adding validation to check user Inputs

```html
<input
type="text"
id="username"
class="form-control"
ngModel
name="username"
required
/>

<input
type="email"
id="email"
class="form-control"
ngModel
name="email"
required
email
/>

<select
id="secret"
class="form-control"
ngModel
name="secret"
required
>

```

- Here v add *required* attribute to the input element.
- if username input not given, form is invalid
- also add *email* directive in email control.
- so if inputted email is not in correct format, then form is invalid.

--- 

## Using the Form state

**app.component.html**
```html
 <!-- valid is a property of form object -->
        <button class="btn btn-primary" type="submit" [disabled]="!form1.valid">
```

- here v disable the button if form is not valid.
- use disabled property - set a condition checking form valid/not.

- when some required inputs are not given or inputted value is not in correct format. then some ng css classes are added to that element.

eg: ng-invalid, ng-dirty, ng-touched

- we can make use of this classes to style form when it is invalid.

**app.component.css**

```css
input.ng-invalid.ng-touched {
  border: 1px solid red;
}
```
- here v add a border to the input when it is invalid.
- ng-touched means we touched that input element but value not added.

---

## Outputting Validation Error Messages

- to access the control created automatically by angular,
- add a local reference to that control - set ngModel directive to that reeference as string 
- use that local refereence in the element we need the access.

**app.component.html**

```html
<div class="form-group">
            <label for="email">Mail</label>
            <input
              type="email"
              id="email"
              class="form-control"
              ngModel
              name="email"
              required
              email
              #email="ngModel"
            />
            <!-- this text appears only if email is inavlid and touched -->
            <span class="text-primary" *ngIf="email.invalid && email.touched"
              >Please add an email</span
            >
          </div>

```
- here v set a local reference in email control 
- used that reference in span element for condition checking.

## set default value with ngModel using property binding


- We want to set a default value to select element when component is initialized.

- for that v do property binding on ngModel and set a property to it

- that property is defined in app.component

**app.component.html**

```html

<select
id="secret"
class="form-control"
[ngModel]="defaultSecretVal"
name="secret"
required
#secret="ngModel"
>
<option value="pet">Your firs`t Pet?</option>
<option value="teacher">Your first teacher?</option>
</select>
```

- here v set *[ngModel]="defaultSecretVal"* in select element

- define this *defaultSecretVal* property in app.component.ts

**app.compoenent.ts**

```typescript
defaultSecretVal: string = "pet"
```
- *pet* value attribute of option tag

---


## Using ngModel with 2 way binding

- we have to instantly show value that is being entered.
 
- use two way binding on ngModel.

- set property to ngModel

**app.component.html**

```html
<div class="form-group">
  <textarea
    name="message"
    id="message"
    cols="100"
    rows="3"
    [(ngModel)]="answer"
  ></textarea>
  <p>
    Your Answer is: <span class="text-danger">{{ answer }}</span>
  </p>
</div>
```

- here entered answer is shown in below paragraph whie it is being entered 

- later set answer property in app component
**app.component.ts**

```typescript
answer = ''
```
---

### summary of diiferent bindings

1. no binding - to tell angular the input is a control
eg: <input ngModel>

2. one way binding - to set a default value to input
eg: [ngModel]="property"

3. two way binding -to show value while it is being entered.
eg: [(ngModel)]="property"

---

## Grouping Form controls

- we can group different controls together.

- use ngModelGroup directive

- set a name to this directive as a string.

- here both the controls were wrapped in to a single div element so we group them.

**app.component.html**

```html
<div id="user-data" ngModelGroup="userData">
  <div class="form-group">
    <label for="username">Username</label>
    <input
      type="text"
      id="username"
      class="form-control"
      ngModel
      name="username"
      required
      #username="ngModel"
    />
    <span
      class="text-primary"
      *ngIf="!username.valid && username.touched"
      >Please add username</span
    >
  </div>
  <button class="btn btn-default" type="button">
    Suggest an Username
  </button>
  <div class="form-group">
    <label for="email">Mail</label>
    <input
      id="email"
      class="form-control"
      ngModel
      name="email"
      required
      email
      #email="ngModel"
    />
    <!-- this text appears only if email is inavlid and touched -->
    <span class="text-primary" *ngIf="email.invalid && email.touched"
      >Please add an email</span
    >
  </div>
</div>
```

- After grouping, v gets a single key: value pair of username and email in value property of form object.

**Screenshot**

![image](./screenshots/Screenshot from 2020-07-07 13-13-02.png 'image')

---

## Handling Radio Buttons

- first we set an array of genders in app.component.ts

**app.component.ts**

```typescript
genders: string[] = ["male", "female"];
// set a defaultGender property
defaultGender: string = this.genders[0];
```

- later create a radio button in html

**app.component.html**

```html
<label for="gender">Select Gender: </label>
<div class="form-check" *ngFor="let gender of genders">
  <input
    class="form-check-input"
    type="radio"
    name="gender"
    id="gender"
    [ngModel]="defaultGender"
    [value]="gender"
  />

  <label for="gender" class="form-check-label">{{ gender }}</label>
</div>

```
- using ngFor, loop thru each data in *genders* array

- for each element, div is repeated.

- we set one way binding on ngModel to set a default value once component loads.

- one way binding on value attribute.

- we specify *gender* in label.

---

## Patching Form Values

- use, *patchValue()* method on form.

- *patchValue()* used to override a parts of the form.


**app.component.ts**

```typescript
suggestUserName() {

    const suggestedName = "Superuser";

    // access form using name that set in @Viewchild - get form object - invoke patchValue - get userData - assign suggestedName to username
    
    this.signupForm.form.patchValue({
    	// get userData
      userData: {
      	// set suggestedName property to username.
        username: suggestedName,
      },
    });
  }
```
**app.component.html**
```html
<button
class="btn btn-default"
type="button"
(click)="suggestUserName()"
>
```
- Here clicking on button, suggest a value for username input control.

---

## Using Form Data













	
