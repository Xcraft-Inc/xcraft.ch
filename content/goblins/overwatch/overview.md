---
title: 'Overview'
date: 2020-09-29
weight: 10
draft: true
---

## Monitor your app

> Know yourself but also know others behavior -- Onyme Anne

Overwatch is a module that allow you to monitor error or hazardous behavior in
your app. To do that you have to set up 2 or 3 things... Don't worry it
shouldn't be too long ;)

{{% notice info %}}It hasn't been designed to be a telemetry service even if it
does the same kind of job (report only error and hazardous behavior). Maybe in
further version it will support more features.{{% /notice %}}

To use the goblin overwatch you have to set up two things :

- Channels = mail adresse(s) and/or discord channel(s)
- (optional) Agent = a cool character from overwatch that show up in discord.
  It's Ana by default but you can customize it for each app :)

First of all, you need to set up channels and an agent in your `app.json` for
the app you want to monitor :

```js
"goblin-overwatch": {
    "channels": {
        "discord": ["xxxxxxxxxxxxxx9768/xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx-HF2aj85YGklWeTZWGeouGiIBueiM4m4Hyuc8"],
        "mail": ["goblin-overwatch@my-domain.ch"]
    },
    "agent": "mccree"
}
```

Once you have done that, at the start of your app, you just need to call the
init quest of overwatch and enable log via the buslog :

```js
    const owAPI = quest.getAPI('overwatch');
    yield owAPI.init();
    yield quest.cmd('buslog.enable', {modes: ['overwatch']});
```
