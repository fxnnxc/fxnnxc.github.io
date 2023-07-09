---
layout: post
title: 'Plotly'
date: 2023-06-14 00:00:00-0400
description: 'Test'
tags: Plotly 
category: PLT
subcategory : Plotly
---


## Basics 

Plotly 
* `plotly.graph_objects as go` 
* `plotly.figure_factory as ff`
* `import plotly.express as px` : make a nice figure 
* `plotly.subplots import make_subplots` : subplots 


* fig : `fig = make_subplots(rows=3, cols=1)`
  *  `.append_trace(graph, row=r, col=c)` 
  *  `.show()`
  *  `.update_layout()`
* trace  : 
  * 
* go : 
  * `Scatter(x,y)` 
  * `Bar(y=y)`

  

## Go

```python
    go.Scatter(
        x=[2, 4],
        y=[4, 8],
        mode="lines",
        line=go.scatter.Line(color="gray"),
        showlegend=False)
    
    go.Bar(y=[2, 1, 3])
```

## Update Layout 

```python 
fig.update_layout(
    height=300, 
    width=600, 
    title_text="Title",
    font_family="Courier New",
    font_color="blue",
    title_font_family="Times New Roman",
    title_font_color="red",
    legend_title_font_color="green"
     )

fig.update_xaxes(title_font_family="Arial")
```

# 