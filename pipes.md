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
- order is relay important, if not maintained , it causes error.

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
