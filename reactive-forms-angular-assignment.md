# Reactive Approach

- create a form in app.component.html

- initialize new form in app.component.ts

**app.component.ts**

```typescript
projectDetailsForm: FormGroup;
statusArray: string[] = ['stable', 'critical', 'finished'];
  ngOnInit() {
    // initialize project details form,
    this.projectDetailsForm = new FormGroup({
      projectName: new FormControl(null, [Validators.required]),
      email: new FormControl(null, [Validators.required, Validators.email]),
      projectStatus: new FormControl(this.statusArray[0]),
    });
```

- next synchronize typescript form with html form.
- connect each inputs with corresponding controls in typescript form using _formControlName_ directive
- add **ngSubmit** directive to submit form.

  **app.component.html**

```html
<form
  [formGroup]="projectDetailsForm"
  class="pb-5 mb-5"
  (ngSubmit)="onSaveProject()"
>
  <div class="form-group">
    <label for="name">Project Name: </label>
    <input
      formControlName="projectName"
      type="text"
      name="name"
      class="form-control"
      id="name"
    />
  </div>

  <div class="form-group">
    <label for="email">Email: </label>
    <input
      formControlName="email"
      type="email"
      name="email"
      id="email"
      class="form-control"
    />
  </div>

  <div class="form-group">
    <div class="col-auto my-1">
      <label class="mr-sm-2 sr-only" for="inlineFormCustomSelect"
        >Status
      </label>
      <select
        class="custom-select mr-sm-2"
        id="inlineFormCustomSelect"
        formControlName="projectStatus"
      >
        <option></option>
      </select>
    </div>
  </div>

  <button class="btn btn-success" type="submit">Submit</button>
</form>
```

> create _onSaveProject()_ function in app.component.ts
> and print the value.

**app.component.ts**

```typescript
 onSaveProject() {
    console.log(this.projectDetailsForm.value);
  }
```

- adding custom validator that doesn't allow project name as **Test**.
-
- create a new file called _customvalidator.ts_
- create a _CustomValidator_ class. create a static method in it.

**customvalidator.ts**

```typescript
export class CustomValidator {
  // define a static method - so can call it
  // without instantiating the class.
  static invalidProjectName(control: FormControl): any {
    // if value received is Test
    if (control.value === 'Test') {
      return { invalidProjectName: true };
    }
    return null;
  }
}
```

- later call this static method in app.component.ts

**app.component.ts**

```typescript
projectName: new FormControl(null, [
  Validators.required,
  CustomValidator.invalidProjectName,
]);
```

> using async validator

**customvalidator.ts**

```typescript
  static asyncInvalidProjectName(
    control: FormControl
  ): Promise<any> | Observable<any> {
    const promise = new Promise<any>((resolve, reject) => {
      setTimeout(() => {
        if (control.value === 'Sample') {
          resolve({ invalidProjectName: true });
        } else {
          resolve(null);
        }
      }, 1500);
    });
    return promise;
  }
```

- then access this validator in app.component.ts

**app.component.ts**

```typescript
projectName: new FormControl(
  null,
  [Validators.required, CustomValidator.invalidProjectName],
  CustomValidator.asyncInvalidProjectName
);
```

- so here basically, if project name is _Test_ or _Sample_ , it shows ng-invalid.
- also async validator should be placed outside the array.

---

**Full Code**:

_app.component.ts_

```typescript
export class AppComponent implements OnInit {
  title = 'reactive-form-project-ver2';
  projectDetailsForm: FormGroup;
  statusArray: string[] = ['stable', 'critical', 'finished'];

  ngOnInit() {
    // Initialize project details form,
    this.projectDetailsForm = new FormGroup({
      projectName: new FormControl(
        null,
        [Validators.required, CustomValidator.invalidProjectName],
        CustomValidator.asyncInvalidProjectName
      ),
      email: new FormControl(
        null,
        [
          Validators.required,
          Validators.email,
          CustomValidator.invalidProjectEmail,
        ],
        CustomValidator.asyncInvalidEmail
      ),
      projectStatus: new FormControl(this.statusArray[0]),
    });
  }
  // form submit
  onSaveProject() {
    console.log(this.projectDetailsForm.value);
  }
  // validate the project name, not allow name 'Test'
  forbiddenName(arg: FormControl): Promise<any> | Observable<any> {
    const promise = new Promise<any>((resolve, reject) => {
      setTimeout(() => {
        if (arg.value === 'Test') {
          resolve({ nameIsForbidden: true });
        } else {
          resolve(null);
        }
      }, 1500);
    });
    return promise;
  }
}
```

_app.component.html_

```html
<div class="container m-5">
  <div class="card">
    <div class="card-body">
      <form
        [formGroup]="projectDetailsForm"
        class="pb-5 mb-5"
        (ngSubmit)="onSaveProject()"
      >
        <div class="form-group">
          <label for="name">Project Name: </label>
          <input
            formControlName="projectName"
            type="text"
            name="name"
            class="form-control"
            id="name"
          />
          <small
            class="text-danger"
            *ngIf="
              projectDetailsForm.get('projectName').invalid &&
              projectDetailsForm.get('projectName').touched
            "
            >fsfsdf</small
          >
        </div>

        <div class="form-group">
          <label for="email">Email: </label>
          <input
            formControlName="email"
            type="email"
            name="email"
            id="email"
            class="form-control"
          />
          <small
            class="text-danger"
            *ngIf="
              projectDetailsForm.get('email').invalid &&
              projectDetailsForm.get('email').touched
            "
            >please add valid mail</small
          >
        </div>

        <div class="form-group">
          <div class="col-auto my-1">
            <label class="mr-sm-2 sr-only" for="inlineFormCustomSelect"
              >Status
            </label>
            <select
              class="custom-select mr-sm-2"
              id="inlineFormCustomSelect"
              formControlName="projectStatus"
            >
              <option *ngFor="let item of statusArray" [ngValue]="item">
                {{ item }}
              </option>
            </select>
          </div>
        </div>

        <button class="btn btn-success" type="submit">Submit</button>
      </form>
    </div>
  </div>
</div>
```

_customvalidator.ts_

```typescript
export class CustomValidator {
  // define a static method - so can call it
  // without instantiating the class.
  static invalidProjectName(control: FormControl): any {
    // if value received is Test
    if (control.value === 'Test') {
      return { invalidProjectName: true };
    } else {
      return null;
    }
  }
  // create an async validator
  static asyncInvalidProjectName(
    control: FormControl
  ): Promise<any> | Observable<any> {
    const promise = new Promise<any>((resolve, reject) => {
      setTimeout(() => {
        if (control.value === 'Sample') {
          resolve({ invalidProjectName: true });
        } else {
          resolve(null);
        }
      }, 1500);
    });
    return promise;
  }
  // custom validator for email - receive the input as argument
  static invalidProjectEmail(control: FormControl): any {
    if (control.value === 'test@test.com') {
      return { invalidEmail: true };
    }
    return null;
  }

  // using async validator
  static asyncInvalidEmail(
    control: FormControl
  ): Promise<any> | Observable<any> {
    const promiseVal = new Promise<any>((resolve, reject) => {
      setTimeout(() => {
        if (control.value === 'sample@sample.com') {
          resolve({ invalidEmail: true });
        } else {
          resolve(null);
        }
      }, 1500);
    });
    return promiseVal;
  }
}
```
