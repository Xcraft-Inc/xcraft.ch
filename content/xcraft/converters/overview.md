---
title: 'Overview'
date: 2020-08-11
weight: 10
tags: ['devel', 'converters']
draft: true
---

![getDisplayed vs. parseEdited](/img/converters.overview.png?classes=blackprint)

## Canonical

![hard canonical](/img/converters.hard.png?classes=blackprint)

**Canonical** values are used by the computer. The format of canonical values is
fixed and rigid. These are the values that are persisted in [RethinkDB][1].

> In computer science, canonicalization (sometimes standardization or
> normalization) is a process for converting data that has more than one
> possible representation into a "standard", "normal", or canonical form. This
> can be done to compare different representations for equivalence, to count the
> number of distinct data structures, to improve the efficiency of various
> algorithms by eliminating repeated calculations, or to make it possible to
> impose a meaningful sorting order.  
> -- [Wikipedia][2]

Generally, canonical values are `string`, except for `number` and `bool`, which
use JS native types. For each type, the string containing the canonical value
respects a precise syntax (see tests in `lib/xcraft-core-converters/test` for
documentation).

Examples of canonical values:

|              |                                                                                                |
| ------------ | ---------------------------------------------------------------------------------------------- |
| **date**     | `"2020-03-31"`                                                                                 |
| **time**     | `"11:30:00"`                                                                                   |
| **datetime** | `"2019-01-18T23:59:59.000Z"`                                                                   |
| **price**    | `"100"`, `"49.95"`                                                                             |
| **delay**    | `"* * * 30 * * *"` _(30 days)_                                                                 |
| **color**    | `"#FF000"` _(red in rgb)_, `"HSL(40,100,100)"`, `"CMYK(100,0,0,50)"`, `G(50)` _( medium gray)_ |

`number` and `bool` use JS native types:

|            |                 |
| ---------- | --------------- |
| **number** | `50`, `0.02`    |
| **bool**   | `true`, `false` |

The function `parseEdited(edited)` parse a free text entered by the user. Some
flexibility allows the user to enter data in various formats, possibly
incomplete, with a minimum of intelligence. For example:

|           |                                                                              |
| --------- | ---------------------------------------------------------------------------- |
| **date**  | `parseEdited("25")` → `"2020-03-25"` _(completed by current month and year)_ |
| **time**  | `parseEdited("12")` → `"12:00:00"` _(completed with zeros)_                  |
| **delay** | `parseEdited("20j")` → `"* * * 20 * * *"`                                    |
| **delay** | `parseEdited("4h")` → `"* * 4 * * * *"`                                      |
| **color** | `parseEdited("#12F")` → `"#1122FF"`                                          |

The function `parseEdited` return a map with `{value, error}`. If everything is
ok, `error === null`. In the event of an `error`, the `value` comes as close as
possible to something plausible.

## Displayed

![soft displayed](/img/converters.soft.png?classes=blackprint)

**Displayed** values are for human users. A displayed value has several possible
representations, more or less long, to adapt to different situations in the UI.

The function `getDisplayed(canonical, format)` format a canonical value to a
string for the human user (the parameter `format` is optional). For example:

|          |                                                                                    |
| -------- | ---------------------------------------------------------------------------------- |
| **date** | `getDisplayed("2020-03-31")` → `"31.03.2020"`                                      |
| **date** | `getDisplayed("2020-03-31", "dMy")` → `"31 mars 2020"` _(with parameter `format`)_ |

See in `lib/xcraft-core-converters/lib` for documentation of each type.

## Many types

There are many types, for all uses:

|              |                                                                                                                                                                  |
| ------------ | ---------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **date**     | A date with day, month and year.                                                                                                                                 |
| **time**     | A time with hours, minutes and seconds.                                                                                                                          |
| **datetime** | A date and time.                                                                                                                                                 |
| **integer**  | An integer, therefore without fractional part.                                                                                                                   |
| **number**   | A real number, therefore with a fractional part.                                                                                                                 |
| **price**    | A price in Swiss francs, with 2 decimal places for cents (accept big numbers).                                                                                   |
| **percent**  | A percentage.                                                                                                                                                    |
| **delay**    | Duration in minutes, hours, days, months or years.                                                                                                               |
| **length**   | A length with different units ("km", "m", "cm", "mm", usw.). The canonical value is in meters.                                                                   |
| **weight**   | A weight with different units ("t", "kg", "g", "mg"). The canonical value is in kilogram.                                                                        |
| **volume**   | A volume defined by 3 lengths, or a number of liters, with different units ("m", "cm", "l", "dm3"). The function `getDisplayedIATA` format a dimensional weight. |
| **color**    | A color in various color space (RGB, HSL, CMYK or gray scale).                                                                                                   |

## Common mistakes

To get today's canonical date, don't do this:

```js
const canonicalNow = date.now();
```

Instead, write this:

```js
const canonicalNow = DateConverters.getNowCanonical();
```

Or, to display the current date in plain text:

```js
const displayedNow = DateConverters.getDisplayed(
  DateConverters.getNowCanonical()
);
```

[1]: https://rethinkdb.com/
[2]: https://en.wikipedia.org/wiki/Canonicalization
