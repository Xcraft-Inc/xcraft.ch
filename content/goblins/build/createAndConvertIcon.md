---
title: 'Create icon'
date: 2020-08-05
weight: 5
tags: ['general', 'goblin', 'build', 'icon']
---

We reccommend you to use a professional vector drawing software to create your
icon like Inkscape (open source) or Adobe Illustrator. Create a new document
with a size of 512x512 pixels and unleash your creativity :)

## Libraries to convert your icon

We are going to use some linux libraries to transform and convert our icon from
`png` to the good extensions allowed by the goblin builder.

We have to generate 3 types of icon for all platforms :

- icon.icns ( for Mac )
- icon.ico ( for Windows )
- icon.svg ( for Linux )

{{% notice warning %}}They will be placed in the folder
`${yourWorkingDirectory}/app/{appName}/icons`. It's important to rename all
files to `icon` or the goblin builder will not find the icon for your app
!{{% /notice %}}

To achieve that, we are going to install 2 libraries from linux :

- icnsutils
- imagemagick

```bash
sudo apt install icnsutils imagemagick
```

{{% notice info %}}If you are on windows, you can use the linux subsystem to
install and use these libraries.{{% /notice %}}

## Convert png to icns

A simple way to convert your icon from `.png` to `.icns` is to use the command
`png2icns` from library icnsutils :

```bash
png2icns icon.icns your-icon.png
```

## Convert png to ico

You can also convert your icon from `.png` to `.ico` by using the command
`convert` from imagemagick. Small detail here, we specify all intermediate sizes
of the icon :

```bash
convert -define icon:auto-resize="256,128,96,64,48,32,16" your-icon.png icon.ico
```
