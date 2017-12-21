---
layout: page
title: How to visualize your data
description: Discover patterns in your data through OpenLattice's core graphing and mapping tools.
---

{% include video.html id="A1y1t8HwGlU" %}

<hr>

* TOC
{:toc}

## Visualization Types

Currently, the supported visualization types are:
* Scatter Chart
* Line Chart
* Map Visualization

We are constantly working to develop new features and visualization types. [Please reach out to us](mailto:{{site.email}}) if there's a common visualization tool you need that we do not currently cover.

## Scatter and Line Charts

These visualizations are useful for representing relationships between
two variables.

Use a **Scatter Chart** when the goal is to explore a trend or
relationship in the data. Scatter Charts are useful for initial exploration
and identifying groups of similar data and outliers.

**Line Charts** can also be used to explore trends, but often represent
trends over time. Line charts connect the points on the graph, so they
should be used to represent continuous data.

For both Scatter and Line Charts, simply choose the numerical properties you want represented on the x-axis and y-axis.

{% include image.html
  caption="Sample Crime Rate vs Crime Index Ranking Scatter Chart"
  path="/guides/visualizations/sample-scatter.png"
%}

## Map Visualization

Use the **Map** visualization when your data contains longitudes
and latitudes. Click on **Map Visualization** to see your data plotted using
OpenStreetMap.

{% include image.html
  caption="Sample Stolen Cars Map Visualization"
  path="/guides/visualizations/sample-map.png"
%}
