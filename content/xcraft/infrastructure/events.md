---
title: 'Events'
date: 2020-08-05
weight: 8
draft: true
---

## Subscribe

When subscribing to a topic for events, a simple syntax can be used in order to
group multiple events in one subscription and without the use of regex.

```js
quest.sub(`*::${somethingId}.(added|updated)`, () => {});
```

This is a complete example which shows all possibilities. The character `*` is a
joker for `[0..n]` characters. The parenthesis can be used for **OR** (with the
pipe `|`) a bit like with a regex.

## Cache

The topics are converted to regex in the low lever layers. These regexes have a
cost because there are thousand of subscriptions and we must be sure that every
received messages are not tested with all registered regexes. A cache organizes
the regexes by small groups in order to reduce drastically the number of calls
on `regex.test`.

In order to have a working cache, it's necessary that the topics use one or more
ID which will match:

```js
/([^.:]+@[^.:]+|\.[a-z0-9]+-[a-z0-9]+-[a-z0-9]+-[a-z0-9]+-[a-z0-9]+\.)/g;
```

The first part of this regex looks like to the goblin ID (high level), and the
second part is corresponding (in principle) to the Xcraft messages ID (low
level).

The cache accesses are done in a **map** where the keys are the extracted IDs.
There is a particular key `_` which contains all regexes that can be potentially
be called by topics with ID too. It's specially the case when a subscription
topic begins with `*`. But note that there is a specific order for the regex
execution in order to prevent to fall too often in the `_` case.
