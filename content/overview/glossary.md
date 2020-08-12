---
title: 'Glossary'
date: 2020-08-11
tags: ['general']
weight: 5
---

![oldmanbook](/img/oldmanbook.svg?width=200px)

> Books are for people who wish they were somewhere else. -- Mark Twain

| Word       | Definition                                                                                                                                  |
| ---------- | ------------------------------------------------------------------------------------------------------------------------------------------- |
| [Axon][1]  | The Message-oriented socket library inspired by zeromq which is used for communicating between processes                                    |
| bus        | Channel for sending/receiving commands and events (there are two buses and it can be in-process or multi-processes)                         |
| Goblin     | An Xcraft module based on the Goblin layer                                                                                                  |
| orcName    | Bus client identifier when connecting to an Xcraft server                                                                                   |
| quest      | Async action (for side effect) dispatched with [Redux][2] (based on the Goblin layer, it's triggered by a generated Xcraft command handler) |
| [Redux][2] | A Predictable State Container for JS Apps (the base of the Goblin layer)                                                                    |
| Xcraft     | Main infrastructure and toolchain                                                                                                           |

[1]: https://github.com/Xcraft-Inc/axon
[2]: https://redux.js.org/
