---
title: 'Modeling'
date: 2020-07-04
weight: 11
---

![plan](/img/plan.png?width=256px)

In the workshop we propose a kind of domain service factory. The domain driven
design guidelines has been a good starting point when we started to define our
first applications (see [DDD][1]). But we have adpated the methodology and rules
to fit a more natural way of designing things. We keep the good of DDD and we
enlight the darkness of evasives points.

We don't pretend to be experts of DDD methodology shared by Eric Evans, we have
read the book and experimented by ourself. The workshop entity builder is a
field experiment, that brought us a better compromise to define domains.

The entity builder can provide service as entities. This uncommon goblins
pattern offer a new way of thinking domain context, root aggregates and
entities. Let's take a look with some "natural" examples.

Before riding this chapter like a cowboy, ensure that you are **ok easy** with
basic goblins concepts.

{{% notice info %}}In the workshop, your modeling choices on paper will make
some noise with other architects and domain experts! Confront your domain
modeling with others, take care of naming. Don't hesite to trash your plan's and
retry with a new iterated model. Continuous delivery of data models is the key
to affine and approach a production ready schema{{% /notice %}}

# example 1

The basic entity, easy to define, hard to name:

```js
const {buildEntity} = require('goblin-workshop');

const entity = {
  type: 'project',
  properties: {
    name: {type: 'string', defaultValue: 'new project'},
  },
  onNew: function (quest, id, name) {
    return {id, name};
  },
};

module.exports = {
  entity,
  service: buildEntity(entity),
};
```

This very simple definition of a **project** entity will produce a loadable
goblin entity service of **type: 'project'** and can be used by other goblins
like that:

```js
const projectAPI = yield quest.createNew('project', {
  desktopId: quest.getDesktop(),
});
```

Has you can see, we simply use **createNew** to instanciate a new entity
service.

{{% notice info %}} The **desktopId** is mandatory, the main reason here is that
the entity service will use it te retreive the current storage {{% /notice %}}

We can also choose the service identifier yourself:

```js
//define your human readable identifier
const projectId = 'project@template';

// use a simple and classic create
const projectAPI = yield quest.create('project', {
  id: projectId, //set you id
  desktopId: quest.getDesktop(),
  name: 'template project'
});
```

With this kind of human readable identifier you can retreive the **template**
project using this kind of create, in a further process, the "name" argument is
useless and can be omitted if the entity service as already persisted the
template.

{{% notice info %}}It's important to keep the service name at start of the
identifier, using something else will produce unexpected results and
errors.{{% /notice %}}

# onNew

The entity is new, and will initialise his state by executing the **onNew:**
function. This function simply return the intial properties value. You will
receive arguments has provided by the caller:

```js
const projectAPI = yield quest.createNew('project', {
  desktopId: quest.getDesktop(),
  name: '007' //initialize the name properties
});
```

If the argument is not provided by the caller, properties will be initialized
with default value (**'new project'** in this case)

If a bad value is provided by the caller, the creation will throw and fail.

```js
//try to create a new project using a number
const projectAPI = yield quest.createNew('project', {
  desktopId: quest.getDesktop(),
  name: 7
});
```

You can also write and use the **onNew** handler like other goblin quests, and
calling another goblin for initializing your project name:

```js
 onNew: function* (quest, id) {
    const finderAPI = quest.getAPI('project-name-finder');
    const name = yield finderAPI.findProjectName();
    return {id, name};
  },
```

See the **onNew** quest like a constructor. It will be called only if the entity
is new, namely the entity has not been created by a goblin before. The goblin
scheduler ensure the transaction. If two goblins try to create the same service
identifier (aka entity), only one create (and only one onNew) will be called,
the second caller will work with the same instance after yielding the create.

# change

```js
yield projectAPI.change({path: 'name', newValue: 'G1-007'});
```

Then, we just call the generic **change({path,newValue})** method, for applying
a single mutation to our entity. Under the hood this mutation will produce some
processing and side effects:

First, the entity will emit a **project@unique-service-id.changed** event. Note
that this event is emitted only if the name has really changed.

In response to this events, some subscribers will start processing the change.
The first consumer of this kind of events is the **entity cache feeder**. Is
role will be explained soon.

If we want to emit a dedicated domain event, we must doing that manually.

```js
//in the projectManager service:
quest.evt('project-name-changed', {oldName: '007', newName: 'G1-007'});
```

Then a email notification service can handle that kind of events (why not?)

[1]:
  https://domainlanguage.com/wp-content/uploads/2016/05/DDD_Reference_2015-03.pdf
