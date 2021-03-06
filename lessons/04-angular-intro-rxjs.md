---
path: "/angular-intro-rxjs"
title: "Angular Intro RxJs"
order: 3
---

<iframe src="https://docs.google.com/presentation/d/13FXIGYriWiH5lUtvSCnbem4nDtK7jH5o8ZGGKC7kgNs/embed?start=false&loop=false&delayms=30000" frameborder="0" width="960" height="569" allowfullscreen="true" mozallowfullscreen="true" webkitallowfullscreen="true"></iframe>

### RxJs is extremely powerful, and at the same time quite complicated

There are a lot of applications of reactive programming that go beyond Angular. RxJs is the JavaScript reactive programming
extension. It is the standard library for reactive programming in JS.

RxJs is based of the observer pattern. It can be used both in NodeJS and the browser and can
help you create very interesting functionalities in your UI.

Test using [stackblitz rxjs playground](https://stackblitz.com/edit/typescript-r5zrww?file=index.ts&devtoolsheight=100)

### Eg: Long click handler

```javascript
import { fromEvent, of } from "rxjs"
import { mergeMap, delay, takeUntil } from "rxjs/operators"

const mousedown$ = fromEvent(document, "mousedown")
const mouseup$ = fromEvent(document, "mouseup")

mousedown$
  .pipe(mergeMap(event => of(event).pipe(delay(700), takeUntil(mouseup$))))
  .subscribe(event => console.log("Long Press!", event))
```

### Eg: Only trap the most recent event(is useful for managing onchange events with API requests)

```javascript
import { fromEvent, interval } from "rxjs"
import { debounce } from "rxjs/operators"

const clicks = fromEvent(document, "click")
const result = clicks.pipe(debounce(() => interval(1000)))
result.subscribe(x => console.log(x))
```

RxJs comes with a variety of operators, these are not all used in Angular, and learning them all
would be enough for another workshop however the RxJs [docs](https://rxjs-dev.firebaseapp.com/)
should give you a one stop shop for trying out new things with reactive programming.

A few things to keep in mind about reactive programming

- everything is a stream
- operators are pure functions
- applications are: responsive, resilient, scalable and message-driven
- even if we are only touching on RxJs, Rx is a library that has multiple other language implementations

### Let's try getting our hands dirty with some code 🤔

### Exercises:

1. fetch a list of urls using RxJs Observables and operators
   _*prerequisites: `npm i xmlhttprequest`*_
   - create a stream of observables from a list of urls
   - use a transform method to capitalize all the content from the response
   - use `mergeMap` to resolve all responses
   - BONUS: can you think of other fun applications?
     what if we used a list of breeds and list the sub species using [this endpoint](https://dog.ceo/dog-api/documentation/sub-breed)

```typescript
import { fromEvent, interval, from, of } from 'rxjs';
import { ajax } from 'rxjs/ajax'
import { map, concatMap, mergeMap, catchError, take, retry, delay } from 'rxjs/operators';
import {XMLHttpRequest} from 'xmlhttprequest';

function* urlGenerator() {
  const urls = ['https://dog.ceo/api/breeds/list/all'];
  for(let idx = 0; idx < urls.length; idx++) {
    yield urls[idx];
  }
}
const getData = url => {
  return ajax({ url })
}
<SOLUTION GOES HERE>
fetchUrl$.subscribe(data => console.log(data))
```

<!--const urls$ = from(urlGenerator()).pipe(take(1))-->

<!--const fetchUrl$ = urls$.pipe(-->
  <!--retry(3),-->
  <!--mergeMap(url => getData(url)),-->
  <!--map(Object.keys(response.message).map(puppy => puppy)),-->
  <!--catchError(error => {
    console.log('error: ', error);
    return of(error);-->
  <!--})-->
<!--)-->
