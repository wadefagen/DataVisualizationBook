---
layout: default
title: "Adding a tooltip \"mouseover\" event to a visualization"
---

# Adding a tooltip mouseover to a visualization

One important aspect of a visualization is to allow a user to dive deeper
into the data that is visualized.  A common and straightforward way to do
this is through `mouseover` events which show additional information about
a data point when the data is either hovered with a mouse or tapped on
a touch-based interface.

The simplest of mouseovers is known as a “tooltip”, a visual popup of
information about the content of the data encoded by the visual element.
[Justin Palmer created a d3.js library](https://github.com/Caged/d3-tip)
that allows for simple tooltips to be displayed easily within d3.js code.


## Adding a tooltip

A tooltip should only be added after a visualization exists and you already
have an understanding of what you want the tooltip to display.  As part of
creating the tooltip, you will need to define the text that will be displayed
in the tooltip.

With a tooltip in mind, there are three steps to add a tooltip to a visual
encoding on your visualization:

1. Creating the d3-tip variable, including initializing the text that will be
   shown in the tooltip.

2. Invoking the d3-tip variable in the context of your visualization.

3. Adding `mouseover` and `mouseout` events to your logic creating your visual
  encodings to control when the tooltip is rendered for the user.


### Step 1: Create the d3-tip variable

A d3-tip variable stores information about the tooltip that will be displayed
when a user interacts with your visualization.  A basic d3-tip variable is
created in the below code:

```js
var tip = d3.tip()
            .attr('class', 'd3-tip')
            .html(function(d) {
              return d["name"];
            });
```

In the above code, the `class` attribute sets a CSS class for the tooltip,
allowing you to control the size, color, and other style features of the tooltip.
The `d3-tip` class has been pre-defined in CSS to make a generic tooltip that
is suitable for most visualizations (don't worry about changing it until
you find it not suiting your needs).

In the above code, the `html` method is invoked when the d3-tip is invoked.
The `d` variable in the inner-function will be the data represented by the
element that is being moused over in the visualization.  The returned value
will be shown, as HTML, in the tooltip.  As such, you may want to include
multiple fields, add HTML formatting, etc:

```js
        .html(function(d) {
          return "The final grade for " + d["name"] + " was " + d["score"];
        });
```


### Step 2: Invoke the d3-tip variable

In order for the d3-tip variable to render its tooltip, it needs to be connected
to your visualization.  This is done by connecting your visualization region
to the d3-tip with the following code:

```js
svg.call(tip);
```

In the above code, `svg` is the name of the variable with your visualization
region (created in your boilerplate code) and `tip` is the name of a d3-tip
variable created in Step 1.  That's it!


### Step 3: Adding `mouseover` and `mouseout`

After the d3-tip variable has been initialized and associated with your
visualization, the last step is to attach an event to the visual element that
will have the tooltip.  As part of the chain of methods describing a visual
element, two new methods must be added:

```js
    .on("mouseover", tip.show)
    .on('mouseout', tip.hide)
```

In the context of a whole visual element, this usually appears at the end of
the chain of methods.  For example, a circle with a tooltip would have
the following structure:

```js
// Pre-existing code:
svg.selectAll("grade")
   .data(data)
   .enter()
   .append("circle")
   .attr("cx", function(d, i) { /* ... */ } )
   // ...
   // ... various other attributes ...
   // ...
   .on("mouseover", tip.show)
   .on("mouseout", tip.hide)
   ;
```

## Example: Adding a tooltip to the "Course Grades" visualization

TODO
