# how angular connects with database?

> don't connect angular to a database directly.
> means don't enter database credentials in angular code. since it is highly insecure. we sent https request to server and also gets http response from the same. on that server, we have nodejs code to interact with database.

![Connect Angular with Database](screenshots/image-1.jpg 'image')

---
