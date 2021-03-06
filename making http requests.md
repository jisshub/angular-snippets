## how angular connects with database?

> don't connect angular to a database directly. means don't enter database credentials in angular code. since it is highly insecure. we sent https request to server and also gets http response from the same. on that server, we have nodejs code to interact with database.
>
> ![Connect Angular with Database](screenshots/image-1.jpg 'image')

---

## anatomy of http request

> most important part of request is _api endpoint_ or _url_.

> means, to where v are sending our requests. api endpoints r like

     /posts/1, /comments/10

> when v r interacting with restful api, will b using _http request method_ like PUT, GET, POST, DELETE.

> need to append _Headers_ to our requests.

> can add a _Body_ to the request.

---

## backend firebase setup

- firebase is not just a db, it's a complete solution that also offers database. ie. a complete backend service.

|| Go to Console > create Project > Real Time Database > Create Database > Start in test mode

> ![Database firebase](screenshots/image-2.jpg 'image')

---

## sending a post request

- in app module import _httpclient_ module from _angular/common/http_.
- also import _FormsModule_ from _angular/forms_
- add it to imports array.

**app.module.ts**

```typescript
import { HttpClientModule } from '@angular/common/http';
imports: [BrowserModule, AppRoutingModule, HttpClientModule, FormsModule];
```

- in app component, inject _httpclientmodule_ to constructor.

**app.component.ts**

```typescript
  constructor(private http: HttpClient) {}

```

> note: in firebase , there is starting url, and then we add the endpoints. this endpoints are replicated as folders in the database. here v r not directly communicating with the db. v r communicating with rest api provided by firebase. firebase translate our endpoints to folder structure and store it in the database. **NEVER COMMUNICATING WITH DB FROM OUR ANGULAR APP**.

```typescript
this.http.post('https://test-angular-fire-project.firebaseio.com/posts.json');
```

- v have to give _json extension_ since it is _firebase requirement_.

here _posts.json_ is the endpoint, remaining are starting url, so folder structure is _posts_.

- next pass request body as second argument.

```typescript
 onCreatePost(postData: { title: string; content: string }) {
    // Send Http request
    this.http.post("https://test-angular-fire-project.firebaseio.com/posts.json", postData)
    console.log(postData);
  }
```

- next v have to **subscribe** to this post method to send the request and to get the response, since _post_ returns an observable and not any response object, but to get response from post request, v have to **subscribe** to the post method. calling _subsribe()_ on _post_ sends the request and gives the actual response data.

**app.component.ts**

```typescript
onCreatePost(postData: { title: string; content: string }) {
    // Send Http request
    this.http.post("https://test-angular-fire-project.firebaseio.com/posts.json", postData)
    .subscribe(responseData => {
     console.log(responseData);
    })
  }

```

- _subscribe_ to post method -> get the response.
- also request r only sent when v _subscribe_ to it.

### response in firebase

![response in firebase](./screenshots/image-3.jpg 'image')

---

## getting the data

- get the data.

**app.component.ts**

```typescript
// create a private method to fetch the posts
  private fetchPosts() {
       this.http.get("https://test-angular-fire-project.firebaseio.com/posts.json").subscribe(posts => {
         console.log(posts);
       })
  }
```

- like post, call _subscribe_ to send the request and get the response back.

- next call this private method,

```typescript
  onFetchPosts() {
    // Send Http request
    this.fetchPosts();
  }
```

---

## using rxjs to transform response data

- here v call _pipe()_ on request method before subscribing it.
- here v use rxjs operator called _map_ operator. import it from _rxjs/operator_
- map operator allows to get some data & return new data that automatically rewrapped  
  into an observable. thus we can subscribe it again. if not observable we cant subscribe the data.

**app.component.ts**

```typescript
  this.http.get("https://test-angular-fire-project.firebaseio.com/posts.json")
       .pipe(map(data =>{
          // converting js object to array of objects.
          // first initialize an empty array
          const postsArray: Array<Object> = [];
          // loop each object in data
          for (const key in data) {
            // push each item as new object to empty array
            postsArray.push({...data[key], id: key})
          }
          return postsArray;
       }))
       .subscribe(posts => {
         console.log(posts);
       })
  }
```

> here v call _pipe_ on get method. inside _pipe_ we call _map_ operator as first argument which allows
> to transform the current data to new form. here v transform from js object to an array of object.
> finally return the mapped data. then call _subscribe_ on the mapped data. log that final data.

---

## Using types with Http Client

- here v add type to the array.
- first create a model.ts file for post

- to create it from cli,

```bash
ng generate class name --type=model
```

**post.model.ts**

```typescript
export class Post {
  constructor(public title: string, public content: string, public id: string) {
    this.title = title;
    this.content = content;
    this.id = id;
  }
}
```

- use this **Post** class as type.

**app.component.ts**

```typescript
const postsArray: Post[] = [];
```

---

## outputting posts

- initialize a empty array with type Post.
- assign mapped posts to that array while subscribing.

  **app.component.ts**

```typescript

loadedPosts: Post[]= [];

.subscribe(posts => {
  this.loadedPosts = posts;

})
```

- later output the data in front end

- first check any data is there.
- if yes, loop thru it.
- finally, print the _title_ and _content_
  **app.component.html**

```html
<div class="col-xs-12 col-md-6 col-md-offset-3">
  <p *ngIf="loadedPosts.length < 1">No posts available!</p>
  <ul class="list-group mt-3" *ngIf="loadedPosts.length >= 1">
    <li class="list-group-item" *ngFor="let post of loadedPosts">
      <h1 class="display-5">{{post.title}}</h1>
      <p class="lead">{{post.content}}</p>
    </li>
  </ul>
</div>
```

---

## using service for http requests

- create service using cli

```bash
ng g s post --skipTests=true
```

- first inject this _PostService_ to the _app component constructor_

**app.component.ts**

```typescript
  constructor(private http: HttpClient, private postService: PostService) {}

```

- in post service, v does **POST and GET** request - store the response data.

- inject http to this service.

**post.service.ts**

```typescript
  constructor(private http: HttpClient) { }

```

- create a function to do post request and store response data.

**post.service.ts**

```typescript
CreateAndStorePost(postData: Post){
    this.http.post("https://test-angular-fire-project.firebaseio.com/posts.json", postData)
    .subscribe(responseData => {
      console.log(responseData);

    })
  }
```

- then in app component file, call this post service.

**app.component.ts**

```typescript

  onCreatePost(postData: Post) {
    // Send Http request
    // call CreateAndStorePost from PostService.
    this.postService.CreateAndStorePost(postData);
  }
```

- similar strategy for _fetching post_ as well.

**post.service.ts**

```typescript
 FetchPosts(){
    this.http.get("https://test-angular-fire-project.firebaseio.com/posts.json")
    .pipe(map(data => {
        const dataArray: Array<Object> = [];
        for (const key in data) {
          console.log(key);

          console.log(data[key]);

          dataArray.push({...data[key], id: key})
        }
        return dataArray;
    }))
    .subscribe(mappedData => {
      console.log(mappedData);

    })
  }
```

**app.component.ts**

```typescript
  onFetchPosts() {
    this.postService.FetchPosts();
  }
```

> but the **problem** here is, we cant view the posts fetched on our template. to solve this, we have to call _subscribe_ method in our _app component_. and not in post service.

**app.component.ts**

```typescript
  onFetchPosts() {
    this.postService.FetchPosts().subscribe(posts => {
      console.log(posts);
      this.loadedPosts = posts;
    });
  }
```

- here v call _subscribe_ on _FetchPosts_ method accessed from _PostService_. later assigned mapped data to the empty array _loadedPosts_.

---

## sending a DELETE request

- delete all posts by sending deleting request.

**post.service.ts**

```typescript
  // delete posts
  DeletePosts(){
    return this.http.delete("https://test-angular-fire-project.firebaseio.com/posts.json")
  }
```

- later subscribe to the returned data in app component.

**app.component.ts**

```typescript

  onClearPosts() {
    // subscribe here, clear the loadedPosts array.
    this.postService.DeletePosts().subscribe(() => {
        this.loadedPosts= [];
    })
  }
```

---

## handling errors

- initialize an error property in app component.
- define a new function to process error.

```typescript
   error = null;

  ngOnInit() {
    this.postService.FetchPosts().subscribe(posts => {
      console.log(posts);
      // assign posts to empty array loadedPosts
      this.loadedPosts = posts;
    }, error => {
      this.error = error.message;
      console.log(error);

    });
  }
```

- render the error in template

**app.component.html**

```html
<div *ngIf="error" class="alert alert-danger mt-3">
  <h2>An Error Occurred</h2>
  <p>{{error}}</p>
</div>
```

---

## using Subjects in handling error.

> we use this method when v send the request and don't subscribe to it in our component.

> in such case we uses _Subject_ to handle the errors.

> create an instance for _Subject_ in post service.

> add a function to process error as argument in _CreateAndStorePost_

> **post.service.ts**

```typescript

  // instance for Subject
  errObject = new Subject<string>();
  constructor(private http: HttpClient) { }

  CreateAndStorePost(postData: Post){
    this.http.post("https://test-angular-fire-project.firebaseio.com/posts.json", postData)
    .subscribe(responseData => {
       console.log(responseData);
    }, error => {
        this.errObject.next(error.message);
    })
  }

```

- declare a property with type as _Subscription_
- subscribe to errObject property and store to errSub property. for that access the post service.

- finally unsubscribe from error, use _ngOnDestroy_ .

- **app.component.ts**

```typescript
  private errSub: Subscription
  ngOnInit() {
    //  subscribe to errObject property and store to errSub property
    this.errSub = this.postService.errObject.subscribe(errMsg => {
        this.error = errMsg;
    })
  ngOnDestroy(){
    // unsubscribe
    this.errSub.unsubscribe();
  }
```

---

## using catchError operator

> import _catchError_ from rxjs operators.
> used when v try to transform the response data using _pipe_.
> in order to throw error caught by _catchError_, use _throwError_ that imported from _rxjs_.
> so _catchError_ followed by _throwError_.

**app.component.ts**

```typescript
FetchPosts(){
    return this.http.get("https://test-angular-fire-project.firebaseio.com/posts.json")
    .pipe(map(data => {
        const dataArray: Array<Object> = [];
        for (const key in data) {
          dataArray.push({...data[key], id: key})
        }
        return dataArray;
    }),
    catchError(errRes => {
      return throwError(errRes);
    }));
  }
```

---

## setting headers

- setting headers for request methods as an argument.

- constructs a new http header with the given values.

```typescript
this.http.get('https://test-angular-fire-project.firebaseio.com/posts.json', {
  headers: new HttpHeaders({ 'Custom-Header': 'Hello' }),
});
```

---

## setting query params in request method

> set params with _HttpParams_ method.
> params passed as argument to _set method_

```typescript
this.http.get('https://test-angular-fire-project.firebaseio.com/posts.json', {
  headers: new HttpHeaders({ 'Custom-Header': 'Hello' }),
  params: new HttpParams().set('print', 'pretty'),
});
```

![image](./screenshots/image-34.jpg 'image')

> can have multiple params in the query

```typescript

 FetchPosts(){
    // create an object for HttpParams
    let searchParams = new HttpParams();
    // append params to that object.
    searchParams = searchParams.append("print", "pretty");
    // can append multiple params as well
    searchParams = searchParams.append("custom", "key");
    // previous params wont be replaced. new ones will be added along with the previous
    return this.http.get("https://test-angular-fire-project.firebaseio.com/posts.json", {headers: new HttpHeaders({"Custom-Header": "Hello"}),
      params: searchParams}
     )
```

![image](./screenshots/image-4.jpg 'image')

---

## observing different types of responses.
