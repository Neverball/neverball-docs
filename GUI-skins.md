#GUI Skinning

Neverball now features a skinnable GUI, which allows GUI designers to give custom look and feel to Neverball's GUI elements.

The default Neverball GUI can be found in `data/gui/`.

##GUI Images

Backgrounds for GUI elements are defined by four common images, each representing a GUI state.

```
gui/back-plain.png
gui/back-plain-focus.png
gui/back-hilite.png
gui/back-hilite-focus.png
```
The `plain` images are used by non-interactive and non-selected GUI elements, while the `hilite` images are used by GUI elements representing selected states, such as the currently used language or the current mouse sensitivity setting. Both of these have `focus` variants, which are used when those elements have focus (via mouse hover, or keyboard/gamepad navigation).

##GUI Description

The way in which the above images are displayed on UI elements is controlled with `gui/desc.txt`, a text based file containing two directives, `slice` and `scale`.

```
slice 0.25 0.25 0.25 0.25
scale 1.0 1.0 1.0 1.0
```

The `slice` directive defines what portion of the GUI images should be used for each border using four parameters. These parameters are ordered left, right, top then bottom, and defined in values between `0.0` (none of the image) and `1.0` (the entirety of the image).

The `scale` directive defines how large the border should be using four parameters. These parameters are ordereed left, right, top then bottom, with `1.0` being the default border width. There is no defined range for these parameters, though negative values will result in GUI elements overlapping, and values above `2.0` may result in GUI elements extending beyond the viewport.

##Distribution

As with other assets, GUI skins may be distributable as a compressed .zip file which can be placed in the user's [`$NEVERBALL_HOME`](Paths.md) folder.
