---
layout: default
title: "Scales"
---

# Scales

Scales are fundamental to every visualization you will create because they allow us to translate data from a data point to a visual encoding.  For example, visualizing a grade on an exam requires the data point (ex: 92%) to be placed on the visualization at the appropriate location to indicate that the grade was a 92%.

d3.js provides numerous built-in scales that covers the most common visual encodings of data.  Your choice of what scale to use will be primary driven by your underlying data:

1. Quantitative data is nearly always in a continuous domain (ex: grades are any value between 0-100, the temperature outside is any value between 0-100, etc).  We’ll explore scales that allow to scale this data to both a continuous range (ex: linearly, logarithmically) and to scale this data to discrete ranges (ex: quantize, quantile).

2. Categorical data contains a set of fixed, discrete values as data points and uses a different set of scales due to the discrete domain.  We’ll explore scales appropriate for bar-chart-style visualizations ("bands") and for single-point visualizations ("points").

Before we get into using scales, we need to understand two fundamental aspects of a scale: domain and range.  If you a familiar with mathematical functions, you will find a scale is nothing more than a mathematical function (y = f(x)) that maps an input domain (x) to a range (y).


## Input Domain

The input domain is the set of values where the scale is defined.  For some scales, the domain is continuous (ex: all possible values between two numbers) or discrete (ex: a set of numbers, categories, or labels).  Continuous domains are often used for quantitative data, where the data is between two fixed points or between a minimum and maximum value.  Discrete domains are used for categorical data, where the data can only be a specific set of values.
d3.js represents all domains, both continuous and discrete, as an array.

### Representing a Continuous Domain in d3.js

Continuous domains are represented by an array of two or more elements.  For example, a grade on an exam (where the exam is scored 0-100) has a continuous from 0 to 100; this is represented by `[0, 100]`.  The GDP per person across various countries may range from $100-$20,000; this is represented by `[100, 20000]`.

### Representing a Discrete Domain in d3.js

Discrete domains are represented by an array of element where each element is a category.  If we have categorized weeks as being “Extremely Dry”, “Dry”, “Average”, “Wet”, and “Extremely Wet”, the domain is the array `[TODO]`.


## Output Range

The output range is the set of values where the scale will map values to from the domain.  While the domain is defined in terms of the input data, the range is defined in terms of the visualization.  For visual encoding of planar features, this range is often in terms of the width or height of the drawing region; for other features, the range may be a color (hue, saturation, etc), value, or other value.

Depending on the scale used, continuous domains may have either continuous or discrete ranges.  d3.js represents a range in the same way as a domain.

## Scale Variables

d3.js stores all information relevant to a specific scale in a scale inside a single JavaScript variable.  To initialize one of these variables, we need three pieces of information:

* The scaling method (ex: linearScale)
* The input domain
* The output range


## Linear Scale

The simplest, and often most useful, scaling method is a linear scale.  A linear scale takes a continuous domain and transforms it to a continuous range using a linear function.  For example, a domain of [0, 10] to a range of [0, 100] would scale input by a factor of 10 (ex: 4 becomes 40, 3.2 becomes 32, etc).

A linearScale is initiated with the `d3.scaleLinear()` method and a domain and range is applied:

```js
var linearScale = d3.scaleLinear()
                    .domain(   /* domain */   )
                    .range(   /* range */    );
```

Linear scales are frequently used and are particularly useful for planar variables, specifically where the data is plotted with on an axis.

## Example 1: A Linear Scale for Grades

The demo visualization "Grades on a Horizontal Axis" uses grades for a ten-student course and places a circle along a horizontal axis to denote how each student performed on an exam.  The input data for this visualization is in the following format:

```json
{ "name": "Alice", "grade": 87 }
{ "name": "Bob", "grade": 63 }
...
```

The grade for each data entry is quantitative data that will always be between the values of 0 and 100 (inclusively).
