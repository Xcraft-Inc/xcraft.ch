---
title: 'get'
date: 2020-07-04
weight: 10
---

Return the full entity state or a sub path. The path arguments supports the
**Shredder** `get` syntax

# Examples

```js

const entityState = yield entityAPI.get();
const name = yield entityAPI.get({path:'name'});
const metaStatus = yield entityAPI.get({path:'meta.status'});

```
