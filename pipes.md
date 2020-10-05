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

## using pipes

- pipes include _uppercase_, _date_.

## adding parameters to pipe

- configuring pipe by adding parameters to it.
- adding multiple parameters to the pipe by separating them with colon.

```html
{{server.startData | date: 'para1':'para2'}}
```

## built-in pipes

- can find _pipes_ in below page.

> [Pipes in Angular](https://angular.io/api?query=pipe)
