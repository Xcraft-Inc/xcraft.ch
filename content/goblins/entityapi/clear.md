---
title: 'clear<>'
date: 2020-07-04
weight: 10
---

Clear a collection

# Example

Remove all `tasks` in collection.

Important: if `tasks` is by value, all values will be hard deleted from storage.

```js
yield entityAPI.clearTasks();
```
