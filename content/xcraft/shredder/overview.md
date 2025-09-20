---
title: 'Overview'
weight: 15
tags: ['devel', 'goblin']
---

## The definition of Immutable

> Unchanging over time or unable to be changed.

![Shredder](/img/goblin-blupi-shredder.png?width=600px&lightbox=false)

Immutable data encourages pure functions (data-in, data-out) and lends itself to
much simpler application development, enabling techniques from functional
programming such as lazy evaluation.

## The shredder

We created the Shredder wrapper to simplify the use of immutable.js in our
applications and for frontend use.

You can shreddify your JS objects by using the Shredder class.

```js
const myShredder = new Shredder({
  id: 'random@213g4auzoaidkhj',
  map: {
    firstElement: {
      subElement: 'test',
    },
    larry: ['1', '2', '3'],
  },
  larray: ['Halle', 'Lujah!'],
});
```

You can still access directly to the immutable.js object by using the key
**`_state`**.

```js
import {Map, fromJS} from 'immutable';

// Pure immutable state
const initialViewState = Map({
  someBackendState: myShredder._state,
  viewStatus: fromJS({
    isPrint: false,
    isLoading: false,
  })
);
```

## The main reason at the beginning

The shredder class was originally created to simplify access to immutable
object. Imagine an object with a deepness of 3 levels :

```js
const imObject = fromJS({
  person: {
    contact: {
      mail: 'test',
    },
  },
});
```

We wanted to avoid doing horrible things like this :

```js
imObject.get('person').get('contact').get('mail');
```

You can simplify the code as below when you use a Shredder object :

```js
imObject.get('person.contact.mail');
```
