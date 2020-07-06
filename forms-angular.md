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

## Template Driven approach
### Creating the form - Registering controls

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

### Submitting the Form 

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



