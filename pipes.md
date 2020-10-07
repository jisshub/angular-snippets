# What are Pipes?

> built in feature that transform output in html template.
> for example,
> suppose we have to render username called 'max' in our template. in such case , v use string interpolation

```typescript
username: string = 'max';
```

```html
<p>{{username}}</p>
```

- but if want to render username in uppercase format in the template. we use **pipes**.

```html
<p>{{username | uppercase}}</p>
```

- here | is the pipe symbol and the name of pipe is _username_.

- so here v use pipe named _uppercase_ to transform the username in our template.

---

## adding parameters to pipe

- configuring pipe by adding parameters to it.
- adding multiple parameters to the pipe by separating them with colon.

```html
<p>{{server.startData | date: 'para1':'para2'}}</p>
```

## built-in pipes

- can find _pipes_ in below page.

> [Pipes in Angular](https://angular.io/api?query=pipe)

## chaining multiple pipes

- when multiple pipes are applied, order is from left to right.

```html
<p>{{server.startData | date: 'fullDate' | uppercase}}</p>
```

- here the _date pipe_ is applied first, then the _uppercase pipe_ is applied to the result of _date pipe_.
- order is really important, if not maintained , it causes error.

---

## creating custom pipe

- create a typescript file _filename.pipe.ts_
- if need of this pipe is to shorten name, then file can be called as _shorten.pipe.ts_.

**shorten.pipe.ts**

```typescript
// import PipeTransform interface
import { PipeTransform } from '@angular/core';
export class ShortenPipe implements PipeTransform {
  // define transform method - receives a value to be transformed
  transform(value: any) {
    // get first 10 characters of the value, use substr() - return shortened value
    return value.substr(0, 10);
  }
}
```

- next to use this returned value,
- add _ShortenPipe_ to _declarations_ array in _app.module.ts_

**app.module.ts**

```typescript
  declarations: [AppComponent, ShortenPipe],

```

- add Pipe decorator - set up a name for pipe.

**app.component.ts**

```typescript
  @Pipe({ name: "shorten" })
  export class ShortenPipe implements PipeTransform {
```

- later use this _shorten_ pipe on template

**app.component.html**

```html
<strong>{{ server.name | shorten }}</strong>
```

- so here server name gets shortened.

- so basically 3 important points to remember

1. add Pipe decorator

```typescript
  @Pipe({ name: "shorten" })
```

2. add transform method.

```typescript
@Pipe({ name: 'shorten' })
export class ShortenPipe implements PipeTransform {
  // define transform method - receives a value to be transformed
  transform(value: any) {
    //   if length of value > 10 characters
    if (value.length > 10) {
      // shorten the value
      return value.substr(0, 10) + '...';
    }
    // if not return original value
    return value;
  }
}
```

3. add pipe to declarations array in app.module.ts file

```typescript
declarations: [AppComponent, ShortenPipe];
```

---

## parametrizing a custom pipe

- adding parameters to pipe.
- suppose we want user to give a value as a parameter to the pipe.
- we have to set an argument in _transform_ method to accept this value.

**app.component.html**

```html
<strong>{{ server.name | shorten: 15 }}</strong>
```

- here v set a parameter 15 to the pipe _shorten_
  **app.component.ts**

```typescript
transform(value: any, limit: number) {
    //   if length of value > 10 characters
    if (value.length > limit) {
      // shorten the value
      return value.substr(0, limit) + "...";
    }
    // if not return original value
    return value;
  }
```

- here v set _limit_ as argument that receives the parameter from pipe.

---

## creating a filter pipe

> filter through the server by status _stable_.
> ie. get only servers with stable status.

- create an input field that takes filtering value.

- using ngModel. allocate a property named _filteredStatus_

- declare the _filteredStatus_ in app component

**app.component.html**

```html
<input
  type="text"
  name="filter-status"
  id="filter-status"
  [(ngModel)]="filteredStatus"
/>
```

**app.component.ts**

```typescript

export class AppComponent {
  filteredStatus = "";
```

- next create a pipe using cli

```bash
ng g pipe filter
```

**filter.pipe.ts**

```typescript
  // give arguments to get entered input and server status
  transform(value: any, filteredString: string, status: string): any {
    // if field input is empty
    if (value.length === 0 || filteredString === "") {
      // display all servers
      return value;
    }
    // define an array to final results
    const resultantArray: Array<string> = [];
    // loop thru each item in value array
    for (const item of value) {
      // check entered value matches current item's status property
      if (item[status] === filteredString) {
        // push the matching item to the resultant array
        resultantArray.push(item);
      }
    }
    // final return back resultant array
    return resultantArray;
  }
```

**app.component.html**

```html
<li
  class="list-group-item"
  *ngFor="let server of servers | filter: filteredStatus:'status'"
  [ngClass]="getStatusClasses(server)"
></li>
```

- give _filter_ pipe - set parameters _filteredStatus_ and _server status_(string)

- make sure filter pipe is added to _declarations_ array in app module.

---

## pure and impure pipes

whenever v make change data on the page, the pipe is recalculated. this might lead to performance issues.
but we can force the pipe to be updated whenever changes happens, it is done by adding a second parameter called
pure to pipe decorator. and set value as false.

**filter.pipe.ts**

```typescript
@Pipe({
  name: "filter",
  pure: false,
})
```

---

## Understanding the async pipe

- create a new property in app component.

```typescript
export class AppComponent {
  appStatus = new Promise((resolve, reject) => {
    setTimeout(() => {
      resolve("stable");
    }, 200);
  });
```

- here _appStatus_ is set to stable only after 2 secs.
- next we have to output this in our template.
- so here v use a built-in pipe called _async_.

```html
<h3>App Status: {{ appStatus | async}}</h3>
```

- so here after 2 secs, _appStatus_ is set to _stable_

- async pipe recognizes the promise and after specified time, promise is resolved and data is printed to the template.

## Assignment

- reverse a string using pipe. - reverse pipe
- sorting the list by server name - sort pipe
