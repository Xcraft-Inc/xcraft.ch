---
title: 'The quest'
date: 2020-08-05
weight: 15
tags: ['devel', 'goblin']
draft: true
---

> Every app has a beginning and an end. What lies between those two points is
> the quest

For defining goblin capabilities, we register a quest to the central agency of
all goblins, that finish in the chief Orc xcraft bus commands registry.

A goblin with a quest named `killJohnFitzgeraldKennedy` can certainly make a
killer feature...

- We register a quest for `goblin-oswald` aka the service name of all instances
- We name the quest
- We provide a function or a generator receiving the `quest` context in first
  argument
- We expect to receive a message containing a date and a place for doing our
  work

```js
Goblin.registerQuest('goblin-oswald', 'killJohnFitzgeraldKennedy', function* (
  quest,
  place,
  date
) {
  //todo: kill the president
});
```
