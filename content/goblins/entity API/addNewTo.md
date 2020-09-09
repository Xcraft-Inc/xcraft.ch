---
title: 'addNewTo<>'
date: 2020-07-04
weight: 10
---

Will create a new value and add it into the collection. This method is generally
called by workitems when you add a new element in [CRUD][1].

This method is generated only for collection of values, ex:

```js
{
  type: 'project',
  values: {
    tasks: 'task[0..n]';
  }
}
```

# Examples

Add a new `task` entity into the `tasks` collection, using default values

```js
yield projectAPI.addNewToTasks();
```

Using a payload for initializing the new `task`

```js
const payload = {
    subject: 'Clean task',
    description: 'Clean codebase'
};

yield projectAPI.addNewToTasks({payload})

```

[1]: https://en.wikipedia.org/wiki/Create,_read,_update_and_delete
