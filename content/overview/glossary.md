---
title: 'Glossary'
tags: ['general']
weight: 10
---

![oldmanbook](/img/overview.glossary.png?width=256px&lightbox=false)

> Books are for people who wish they were somewhere else. -- Mark Twain

| Word       | Definition                                                                                                                                  |
| ---------- | ------------------------------------------------------------------------------------------------------------------------------------------- |
| [Axon][1]  | The Message-oriented socket library inspired by zeromq which is used for communicating between processes                                    |
| bus        | Channel for sending/receiving commands and events (there are two buses and it can be in-process or multi-processes)                         |
| Goblin     | An Xcraft module based on the Goblin layer                                                                                                  |
| orcName    | Bus client identifier when connecting to an Xcraft server                                                                                   |
| quest      | Async action (for side effect) dispatched with [Redux][2] (based on the Goblin layer, it's triggered by a generated Xcraft command handler) |
| [Redux][2] | A Predictable State Container for JS Apps (the base of the Goblin layer)                                                                    |
| [watt][3]  | A Powerful control flow using generators (an other name is `gigawatts` because the project is forked); it's like `async` / `await`          |
| Xcraft     | Main infrastructure and toolchain                                                                                                           |

[1]: https://github.com/Xcraft-Inc/axon
[2]: https://redux.js.org/
[3]: https://github.com/Xcraft-Inc/gigawatts
