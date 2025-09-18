---
title: 'apply'
date: 2020-07-04
weight: 10
---

Apply a new state to an entity.

## Example

```js
const patch = {
  score: 110,
  lvl: 10,
  powerUpStatus: 'enabled',
};
yield entityAPI.apply({patch});
```
