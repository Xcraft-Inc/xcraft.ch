---
title: 'Make your own gadgets'
date: 2020-07-24
weight: 40
draft: true
---

## Make your own gadgets

![smile](/img/smile5.png?width=250px)

We will create a `smiley` widget that displays a satisfaction rating from a
value between 0 (unhappy) and 100 (happy). To do this, you have to create a new
`smiley` folder in the `widgets` folder of your app, which will contain 2 files:

| File                       | Content                   |
| -------------------------- | ------------------------- |
| `widgets/smiley/widget.js` | Class of the widget.      |
| `widgets/smiley/styles.js` | Styles CSS of the widget. |

The skeleton of our file `widgets/smiley/widget.js` should contain this:

```jsx
import React from 'react';
import Widget from 'goblin-laboratory/widgets/widget';
import * as styles from './styles';

export default class Smiley extends Widget {
  constructor() {
    super(...arguments);
    this.styles = styles;
  }

  render() {
    return null;
  }
}
```

To get started simply, let's create a widget that shows a yellow circle:

```jsx
render() {
    return <div className={this.styles.classNames.smiley} />
}
```

The `widgets/smiley/styles.js` file must contain the CSS style named `smiley`:

```jsx
export default function styles() {
  const smiley = {
    width: '200px',
    height: '200px',
    borderRadius: '200px',
    border: '10px solid black',
    background: 'yellow',
  };

  return {
    smiley,
  };
}
```

{{% notice warning %}} The value of `200px` for the radius is not an error, but
a simplification. Logically, we should give half the diameter (`100px`) and
adding the thickness of the border (`10px`), but luckily, CSS truncates the
value if it is too large, and we therefore get a circle. {{% /notice %}}

{{% notice info %}} The styles management uses a cache for more efficiency, with
the lib [aphrodite](https://github.com/Khan/aphrodite). {{% /notice %}}

In the parent widget that instantiates the smiley, you have to write this:

```jsx
import Smiley from 'goblin-gadgets/widgets/smiley/widget';

...
render() {
    return (
        <Container>
            ...
            <Smiley />
            ...
        </Container>
    )
}
```

And unsurprisingly, we get the following result:

![step 1](/img/gadgets.doit.step1.png?width=200px)

## With eyes and a smile

Let's add two eyes and a smile in `widgets/smiley/widget.js`:

```jsx
render() {
    return (
        <div className={this.styles.classNames.smiley}>
            <div className={this.styles.classNames.leftEye} />
            <div className={this.styles.classNames.rightEye} />
            <div className={this.styles.classNames.smile} />
        </div>
    )
}
```

In `widgets/smiley/styles.js`, we need to add 3 new CSS styles `leftEye`,
`rightEye` and `smile`:

```jsx
export default function styles() {
  const smiley = {
    position: 'relative',
    width: '200px',
    height: '200px',
    borderRadius: '200px',
    border: '10px solid black',
    background: 'yellow',
  };

  const _eye = {
    position: 'absolute',
    top: '60px',
    width: '20px',
    height: '20px',
    borderRadius: '20px',
    backgroundColor: 'black',
    transform: 'scaleY(2)',
  };

  const leftEye = {
    ..._eye,
    left: '70px',
  };

  const rightEye = {
    ..._eye,
    right: '70px',
  };

  const smile = {
    position: 'absolute',
    bottom: '40px',
    left: '30px',
    right: '30px',
    height: '60px',
    borderRadius: '0 0 150px 150px / 0 0 150px 150px',
    border: '10px solid black',
    borderTop: 0,
  };

  return {
    smiley,
    leftEye,
    rightEye,
    smile,
  };
}
```

{{% notice info %}} It's a good practice to name the first style (that of the
root `div`) with the same name as the widget (here `smiley`). This will make it
easier to find the widget when you inspect the DOM. The styles of the children
(here `leftEye`, `rightEye` and `smile`) can be named freely. {{% /notice %}}

We obtain the following magnificent result:

![step 2](/img/gadgets.doit.step2.png?width=200px)

## The properties

Our smiley has a fixed size of `200px`. It would be cool to be able to specify
its size using a property, and allow this to be written in the parent widget
which instantiates the smiley:

```jsx
render() {
    return <Smiley size="500px" />
}
```

For that, we must improve `widgets/smiley/styles.js` by declaring the `size`
property:

```jsx
export const propNames = ['size'];

export default function styles(theme, props) {
  const smiley = {
    width: props.size,
    height: props.size,
    borderRadius: props.size,
    border: '10px solid black',
    background: 'yellow',
  };

  return {
    smiley,
  };
}
```

If the `size` property is omitted, it is useful to have a default value. We can
write this:

```jsx
export const propNames = ['size'];

export default function styles(theme, props) {
  const {size = '200px'} = props;

  const smiley = {
    width: size,
    height: size,
    borderRadius: size,
    border: '10px solid black',
    background: 'yellow',
  };

  return {
    smiley,
  };
}
```

## And more...

With a little bit of work in the CSS, you can achieve this:

![step 3](/img/gadgets.doit.step3.png?width=200px)

In the final widget, it will be possible to add a `satisfaction` property (we
see here the results with the values `100`, `75`, `50`, `25` and `0`):

![step 4](/img/gadgets.doit.step4.png)

Why not a `color` property?

![step 5](/img/gadgets.doit.step5.png)

To obtain these variants, just write:

```jsx
render() {
    return (
        <Container>
            <Smiley size="200px" satisfaction={100} mainColor="#0ff" />
            <Smiley size="200px" satisfaction={100} mainColor='#f0f' topColor='#0ff' />
            <Smiley size="200px" satisfaction={75} mainColor='#08f' topColor='#0ff' />
            <Smiley size="200px" satisfaction={25} mainColor='#0a0' topColor='#ff0' />
            <Smiley size="200px" satisfaction={0} mainColor='#fa0' topColor='#f00' />
        </Container>
    )
}
```

You will find the definitive widget in `goblin-gadgets/widgets/smiley`. It is
possible to test it with [WidgetDoc](/goblins/gadgets/widgetdoc).

![WidgetDoc](/img/gadgets.doit.widgetdoc.png)
