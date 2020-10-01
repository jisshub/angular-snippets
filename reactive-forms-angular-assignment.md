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
- connect each inputs with corresponding controls in typescript form.
- add **ngSubmit** directive to submit form.

  **app.component.html**

```html
<form
  [formGroup]="projectDetailsForm"
  class="pb-5 mb-5"
  (ngSubmit)="onSubmit()"
>
  <div class="form-group">
    <label for="name">Project Name: </label>
    <input
      formControlName="name"
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
        formControlName="status"
      >
        <option></option>
      </select>
    </div>
  </div>

  <button class="btn btn-success" type="submit">Submit</button>
</form>
```
