# Create Component

```bash
ng g c about --skipTests=true
```

**about/about.component.html**

```html
<h3>About this Page</h3>
```

**app/app.component.html**

```html
<a [routerLink]="['about', 'me']">About</a>
```

**app.module.ts**

```typescript
{path: "about/me", component: AboutComponent}
```

