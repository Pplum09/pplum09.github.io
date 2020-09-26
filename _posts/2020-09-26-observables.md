---
layout: post
title: A Lazy Guide to RxJS Observables
date: 2020-09-26 05:00:00
tags: rxjs observables angular
author: Izzy
published: true
---

RxJS Observables are confusing, but they don't have to be.
This is not the guide to follow if you want super technical terms.
This is guide is for people like me.
People that just want the answers without getting deep into the details.
The details will come after you've gotten your feet wet.

#### A Tank Full of Water
Observables hold water, or rather data.
When we see a water tower in the distance, we know that it _can_ hold water.
However we aren't entirely sure if it currently has water in it or not.
```
// Read as a variable named tank of type Observable that holds type Water,
// where type Water is defined somewhere else.

public tank: Observable<water> = someWaterSource;
```

#### The Faucet
So how can we extract water out of our tower?
We need to open the faucet and let all of that water pour out.
That's where `subscribe()` comes in.
```
tank.subscribe(bucket => {
  // do something to bucket
});
```
Wait a minute this looks like a _callback function_.
If you're not entirely sure what that is, head on over to my other post on [asychronous functions](https://pplum.io/2018/05/15/sync-vs-async.html).

#### So What Happened?
When we subscribed to the observable, we __consumed__ the resource that it contained.
The resource ended up in the callback function passed into the `subscribe()` function.
At this point the water tank contains no more water because we have just let it all out (into a bucket variable)!

#### Real World Example
In your Angular project, you've probably had to make a request to the server via the `HttpClient` library.
Like our example, this library conveniently returns an Observable type as well.
```
// HttpClient defined as http in constructor above

const url = 'https://pokeapi.co/api/v2/type/11/';
public tank: Observable<Water> = this.http.get<Water>(url);
```
The code above just defines __which__ water tower we are turning the faucet on for.
In this case, we are hitting the pokeapi for water types.
Note that at this point in time, no http request has been sent out to the server yet.
This will only occur when we subscribe / turn on the faucet.

```
tank.subscribe(response => {
  console.log(response);
});
```
You can see above that we have now consumed the pokeapi and have received its response within the `subscribe()` function.
Note that outside of the context of this function, the response does not exist.
Since it is asynchronous, we can't even guarantee when the API will respond.
What we do know is that whenever the API responds, the `subscribe()` function callback will be right there waiting.
To see a live example you can click on the StackBlitz [here](https://stackblitz.com/edit/angular-ivy-mzsyx2?file=src/app/app.component.ts).

I hope this article helped you to learn more about RxJS Observables than it did water tanks.
If you have any additional questions, feel free to shoot me an email.
Thanks for reading!

