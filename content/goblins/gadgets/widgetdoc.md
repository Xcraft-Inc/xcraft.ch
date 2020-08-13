---
title: 'Test gadgets for real'
date: 2020-07-24
weight: 30
draft: true
---

## Test gadgets for real

`WidgetDoc` is a great tool for experimenting with most widgets. You choose the
widget and then interactively define its properties. You immediately see the
graphical result and the code to write to obtain this result. Nice, isn't it?

Most of the apps created have a [Desktop](/goblins/desktop) which contains a
**WidgetDoc** button at the bottom left:

![Call WidgetDoc](/img/widgetdoc.call.png?width=600px)

{{% notice info %}} `WidgetDoc` is itself a [widget](/goblins/gadgets).  
If your app doesn't have a **WidgetDoc** button, you can create a dialog and
drop a `WidgetDoc` widget inside. {{% /notice %}}

On the first run, the window looks like this:

![Call WidgetDoc](/img/widgetdoc.run.png)

The interface shows 3 columns:

![WidgetDoc and button](/img/widgetdoc.button.png)

| Column     | Function                                                                                                      |
| ---------- | ------------------------------------------------------------------------------------------------------------- |
| WIDGETS    | Widget choice.                                                                                                |
| PROPERTIES | Defining properties. Each widget has its own list.                                                            |
| PREVIEW    | Settings for the display of the widget (without influence on its properties), and preview of Code and widget. |

### Column **WIDGETS**

Simply choose the widget to test.

{{% notice info %}} If the desired widget is not in the list, it does not
contain the files `props.js` and `senarios.js`. {{% /notice %}}

### Column **PROPERTIES**

The **filter** is used to display only properties containing a given keyword,
including in enumerations.

The properties are listed in groups. If you prefer to see them in alphabetical
order, deactivate the small button **Cube3D**.

The `Scenarios` define the set of properties, to achieve a frequently used
effect.

![One property](/img/widgetdoc.property.png?width=800px)

| Rank | Function                             |
| ---- | ------------------------------------ |
| 1    | Property name.                       |
| 2    | Current value of property.           |
| 3    | Show a combo with samples of values. |
| 4    | Clear the property.                  |
| 5    | Type of property.                    |
| 6    | `optional` or `required`.            |

Some properties can be of several different types. They have a small button
**Triple ellipsis** which allows to choose the type:

![Multi-types property](/img/widgetdoc.types.png?width=500px)

### Column **PREVIEW**

This column has 3 blocks (CODE, SETTINGS and the name of the tested widget). The
first shows the CODE to use to get a widget looking visible in the last block.

![Preview](/img/widgetdoc.preview.png?width=500px)

The SETTINGS block allows you to choose how the widget is displayed. It does not
modify the properties of the widget, it acts on the parent widget which contains
the tested widget.

The row "Container **resizable**" is useful for testing the behavior of the
widget when the parent's dimensions change:

![Resizable](/img/widgetdoc.resizable.png?width=500px)
