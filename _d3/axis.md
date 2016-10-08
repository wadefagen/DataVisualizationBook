---
layout: default
title: "Adding an axis and gridlines to a visualization"
---

# Axis and Axes

In almost every visualization, at least one axis is used.  An axis provides
a representation of a scale through value label, tick marks, and gridlines.
When you have a [pre-existing d3.js scale](scale.html), axes are simple
to implement in a visualization using d3.js.


## Adding an Axis



### Encoding axis parameters into an axis variable

Every d3.js axis requires an orientation and a [d3.js scale](scale.html).

The orientation of the axis determines if the axis is drawn vertically or
horizontally.  A vertical axis can be further orientated to have tick marks
extending left from the axis (`axisLeft`) or right from the axis (`axisRight`);
a horizontal axis can have ticks extend above (`axisTop`) or below (`axisBottom`)
the axis.

Given an orientation and a scale (`scale`, in the example below), the
following JavaScript code creates a variable encoding an axis:

```js
var axis = d3.axisLeft()
             .scale(scale);
```


### Drawing the axis on the visualization

After an axis variable is created, the axis must be drawn on the visualization.
As an axis contains numerous individual elements that we may want to apply
effects to as a group, an axis is drawn within a `<g>` tag within the SVG.

With a drawing region variable `svg` and axis variable `axis`, the following
code adds the axis to a visualization:

```js
// Draw an axis
svg.append("g")
   .call(axis);
```

An axis is always drawn from (0, 0) and will extend the distance specified
by the underlying scale variable.  This may result in the need to customize
your axis to fit your visualization (ex: to position the axis at the bottom,
to extend a margin to show all the axis labels, etc).


## Customizing Axes

### Translating the Position of the Axis

As noted earlier, an axis is always drawn from (0, 0).  For `axisRight` and
`axisBottom` oriented tick marks, it is often desirable for these axes to
be positioned on the right or bottom of the visualization.

To accomplish this, we can translate the group that the axis is drawn in.
This is done by adding a `transform` attribute to the `g` element that
draws the axis.

To move the axis to the right side, the transformation is to move the axis
group by `(width, 0)`.  The new code to draw the axis is now:

```js
// Draw an axis on the right side of a visualization
svg.append("g")
   .attr("transform", "translate(" + width + "," + 0 + ")")
   .call(axis);
```


Similarly, to move the axis to the bottom, the transformation is to move the
axis by `(0, height)`.  The new code to draw the axis is now:

```js
// Draw an axis on the right side of a visualization
svg.append("g")
   .attr("transform", "translate(" + 0 + "," + height + ")")
   .call(axis);
```


### Adding gridlines

In some visualizations, gridlines are important to show an axis label
throughout a full visualization instead of only at the tick mark.

d3.js does not directly support gridlines, but drawing them is a simple
extension of a tick from a axis.  To extend the tick to make a gridline,
we will create a new, custom axis that is identical to the original
axis except that we must:

1. Disable the display of the tick value, so no text is visible.

2. Change the size of the tick to be negative by the amount of space we
   want the gridlines to extend.

3. Apply a CSS class when drawing the gridline to allow the gridline
   to be styled as a whole (ex: gridlines are usually a lighter color
   than an axis).


To accomplish (1) and (2), a new axis is created that is identical to our
original axis with two extra properties:

```js
var gridlines = d3.axisLeft()        // Same orientation as the axis that needs gridlines
                  .tickFormat("")    // (1): Disable the text for the gridlines
                  .tickSize(-width)  // (2): Extend the tick `width` amount, negative
                  .scale(scale);     // Same scale as the axis that needs gridlines
```

To accomplish (3), the next axis must be drawn with a `class` attribute:

```js
svg.append("g")
   .attr("class", "grid")   // (3): Add a CSS class
   .call(gridlines);
```

Finally, a CSS class needs to be added to match.  For example, the following
can be added to your HTML page:

```html
<style>
.grid line { stroke: #ddd; }
</style>
```
