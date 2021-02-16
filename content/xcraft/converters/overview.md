---
title: 'Overview'
date: 2020-08-11
weight: 10
tags: ['devel', 'converters']
---

![getDisplayed vs. parseEdited](/img/converters.overview.png?width=800px)

## Canonical

![hard canonical](/img/converters.hard.png?width=200px)

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

Generally, canonical values are `string`, except for `number` and `integer`,
which use JS native types. For each type, the string containing the canonical
value respects a precise syntax (see tests in `lib/xcraft-core-converters/test`
for documentation).

Examples of canonical values:

| Type     | Example                                                                                        |
| -------- | ---------------------------------------------------------------------------------------------- |
| date     | `"2020-03-31"`                                                                                 |
| time     | `"11:30:00"`                                                                                   |
| datetime | `"2019-01-18T23:59:59.000Z"`                                                                   |
| price    | `"100"`, `"49.95"`                                                                             |
| delay    | `"* * * 30 * * *"` _(30 days)_                                                                 |
| color    | `"#FF000"` _(red in rgb)_, `"HSL(40,100,100)"`, `"CMYK(100,0,0,50)"`, `G(50)` _( medium gray)_ |

`number` and `integer` use JS native types:

| Type    | Example      |
| ------- | ------------ |
| number  | `50`, `0.02` |
| integer | `450`, `-3`  |

The function `parseEdited(edited)` parse a free text entered by the user. Some
flexibility allows the user to enter data in various formats, possibly
incomplete, with a minimum of intelligence. For example:

| Type  | Example                                                                      |
| ----- | ---------------------------------------------------------------------------- |
| date  | `parseEdited("25")` → `"2020-03-25"` _(completed by current month and year)_ |
| time  | `parseEdited("12")` → `"12:00:00"` _(completed with zeros)_                  |
| delay | `parseEdited("20j")` → `"* * * 20 * * *"`                                    |
| delay | `parseEdited("4h")` → `"* * 4 * * * *"`                                      |
| color | `parseEdited("#12F")` → `"#1122FF"`                                          |

The function `parseEdited` return a map with `{value, error}`. If everything is
ok, `error === null`. In the event of an `error`, the `value` comes as close as
possible to something plausible.

## Displayed

![soft displayed](/img/converters.soft.png?width=200px)

**Displayed** values are for human users. A displayed value has several possible
representations, more or less long, to adapt to different situations in the UI.

The function `getDisplayed(canonical, format)` format a canonical value to a
string for the human user (the parameter `format` is optional). For example:

| Type | Example                                                                            |
| ---- | ---------------------------------------------------------------------------------- |
| date | `getDisplayed("2020-03-31")` → `"31.03.2020"`                                      |
| date | `getDisplayed("2020-03-31", "dMy")` → `"31 mars 2020"` _(with parameter `format`)_ |

See in `lib/xcraft-core-converters/lib` for documentation of each type.

## Many types

There are many types, for all uses:

| Type     | Use                                                                         | Canonical                    | Displayed                                              |
| -------- | --------------------------------------------------------------------------- | ---------------------------- | ------------------------------------------------------ |
| date     | A date with day, month and year.                                            | `"2020-03-31"`               | `"31.03.2020"` `"31 mars 2020"` `"Mardi 31 mars 2020"` |
| time     | A time with hours, minutes and seconds.                                     | `"13:20:30"`                 | `"13:20"`                                              |
| ...      |                                                                             | `"01:30:45"`                 | `"1 heure 30"`                                         |
| datetime | A date and time.                                                            | `"2019-01-18T12:48:00.000Z"` | `"18.01.2019 12:48"`                                   |
| number   | A real number, therefore with a fractional part.                            | `2.003`                      | `"2.003"`                                              |
| integer  | An integer, therefore without fractional part.                              | `45`                         | `"45"`                                                 |
| price    | A price in Swiss francs, with 2 decimal places for cents (use big numbers). | `"29.9"`                     | `"29.90"`                                              |
| percent  | A percentage in range [0..1].                                               | `"0.52"`                     | `"52%"`                                                |
| delay    | A duration in minutes, hours, days, months or years.                        | `"* * 4 * * * *"`            | `"4h"`                                                 |
| ...      |                                                                             | `"* * * 3.5 * * *"`          | `"3.5j"`                                               |
| length   | A length in meters (displayed in "km", "m", "cm" or "mm").                  | `"0.045"`                    | `"0.045m"` `"4.5cm"` `"45mm"`                          |
| weight   | A weight in kilogram (displayed in "t", "kg", "g" or "mg").                 | `"0.123"`                    | `"0.123kg"` `"123g"`                                   |
| volume   | A volume defined by 3 lengths in meters (displayed in "m", "cm" or "mm").   | `"0.12 0.13 1.4"`            | `"12 × 13 × 140 cm"`                                   |
| ...      | A volume defined by 1 value in meters (displayed in "l" or "dm3").          | `"0.012"`                    | `"12dm3"` `"12l"`                                      |
| ...      | The function `getDisplayedIATA` format a dimensional weight.                | `"1 1 1"`                    | `"167kg"`                                              |
| color    | A color in various color space (RGB, HSL, CMYK or gray scale).              | `"#FF0000"`                  | `"#FF0000"`                                            |
| ...      |                                                                             | `"CMYK(0,0,0,0)"`            | `"CMYK(0,0,0,0)"`                                      |

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
