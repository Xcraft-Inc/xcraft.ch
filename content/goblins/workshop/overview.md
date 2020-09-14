---
title: 'Overview'
date: 2020-07-04
weight: 10
---

The goblin workshop is made for crafting and modeling typical [CRUD][1] app
domains. It's a rapid tools for iterate data models, bind a UI. For mastering
this powerfull layer, you must understand the key concepts and best pratices.

Combined with some other modules provide a full framework of goblins. We hope
that this crafting experience will make you gain a precious times.

# Key concepts

- workshop contains **services factories** for building, searching and editing
  entities graphs
- workitems services permit **easy CRUD** operations on entities graph
- entities are goblin's service like others
- entities has **auto-generated** [quests] for CRUD operations
- entities has aggregation functions for summaries, computing and indexing

# Dependencies and side modules

The workshop module cover a mid-level data layer and he gets along well with his
childhood friends.

| goblin module          | main purpose                                         |
| ---------------------- | ---------------------------------------------------- |
| xcraft-core-converters | provide data types and canonical value to store      |
| goblin-destkop         | provide workitem UI and workitems managements        |
| goblin-gagdets         | provide data binding UI components                   |
| goblin-rethink         | cache storage adapter for entities                   |
| goblin-elasticsearch   | indexer and search engine for entities               |
| goblin-cryo            | root and backup storage, provide history on entities |
| goblin-nabu-storage    | translations messages storage                        |

Your modules will provide entities and workitems, don't hesite to split domains
in separate goblins modules. We encourage composable and reusable domains.
Overloading domains

[1]: https://en.wikipedia.org/wiki/Create,_read,_update_and_delete
