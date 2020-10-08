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

