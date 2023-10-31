---
title: Data Visualization and manipulation using Bookeh
author: shaukat
date: 2023-October-30 11:33:00 +0800
categories: [assignment, finance, interactive]
tags: [data visualization]
pin: true
math: true
mermaid: true
image:
  path: /commons/devices-mockup.png
  lqip: data:image/webp;base64,UklGRpoAAABXRUJQVlA4WAoAAAAQAAAADwAABwAAQUxQSDIAAAARL0AmbZurmr57yyIiqE8oiG0bejIYEQTgqiDA9vqnsUSI6H+oAERp2HZ65qP/VIAWAFZQOCBCAAAA8AEAnQEqEAAIAAVAfCWkAALp8sF8rgRgAP7o9FDvMCkMde9PK7euH5M1m6VWoDXf2FkP3BqV0ZYbO6NA/VFIAAAA
  alt: Responsive rendering of Chirpy theme on multiple devices.
---

# Data Visualization and manipulation using Bookeh

Bokeh is a Python library for creating web apps with interactive data visualizations.

The following code shows different implementation of the bookeh ot crate vsiaialiton for a dataset of the Fortune 500 Companies 1955-2021.

Here are some intro to the dataset from [Kaggle][1]:

## **About Dataset**

Since 1955, Fortune magazine has published the top 500 companies in the US based on annual revenue. This list shows all 500 for each year. Note: 2015 is missing revenue figures, I have been unable to find them - contact me if you have them and I'll add.

This data was used to create these quizzes:
https://hugequiz.com/quizzes/fortune-500-top-25-by-year/
https://hugequiz.com/quizzes/fortune-500-companies-choose-number/
 
### Below is an excerpt from the dataset and how it looks and represents the data.

| Year | Company Name       | Revenue ($) | Rank |
| ---- | ------------------ | ----------- | ---- |
| 2021 | Walmart            | 559151      | 1    |
| 2021 | Amazon             | 386064      | 2    |
| 2021 | Apple              | 274515      | 3    |
| 2021 | CVS                | 268706      | 4    |
| 2021 | UnitedHealth Group | 257141      | 5    |
| 2021 | Berkshire Hathaway | 245510      | 6    |
| 2021 | McKesson           | 231051      | 7    |
| 2021 | AmerisourceBergen  | 189893.9    | 8    |
| 2021 | Alphabet           | 182527      | 9    |
| 2021 | Exxon Mobil        | 181502      | 10   |

## Code Implementation

	from bokeh.io import show, output_file
	from bokeh.models import ColumnDataSource, HoverTool
	from bokeh.plotting import figure
	from bokeh.transform import factor_cmap
	from bokeh.palettes import Spectral11
	from bokeh.io import push_notebook, output_notebook, show
	
	
	# Data
	companies = ["Walmart", "Amazon", "Apple", "CVS", "UnitedHealth Group", "Berkshire Hathaway", "McKesson", "AmerisourceBergen", "Alphabet", "Exxon Mobil", "AT&T", "Costco"]
	revenues = [559151, 386064, 274515, 268706, 257141, 245510, 231051, 189893.9, 182527, 181502, 171760, 166761]
	ranks = list(range(1, 13))
	
	source = ColumnDataSource(data=dict(companies=companies, revenues=revenues, ranks=ranks))
	
	# Plot
	p = figure(y_range=companies, height=700, title="Company Revenues (2021)",
	           toolbar_location=None, tools="")
	
	
	p.hbar(y='companies', right='revenues', height=0.4, source=source,
	       color=factor_cmap('companies', palette=Spectral11, factors=companies))
	
	# Add Hover Tool
	hover = HoverTool()
	hover.tooltips = [("Revenue", "@revenues{$0.2f}"), ("Rank", "@ranks")]
	p.add_tools(hover)
	
	p.ygrid.grid_line_color = None
	p.ygrid.minor_grid_line_color = None
	p.xgrid.minor_grid_line_color = None
	
	p.outline_line_color = None
	p.xaxis.axis_label = "Revenue (in thousands)"
	p.yaxis.axis_label = "Companies"
	
	output_file("revenue.html")
	output_notebook()


## Output for the saample data

<iframe src="/assets/img/bokeh/Revenue Sample.html"   
    sandbox="allow-same-origin allow-scripts"
    width="100%"
    height="500"
    scrolling="no"
    seamless="seamless"
    frameborder="0">
</iframe>

## Output for the saample data

<iframe src="/assets/img/bokeh/'revenue basic plot.html"   
    sandbox="allow-same-origin allow-scripts"
    width="100%"
    height="500"
    scrolling="no"
    seamless="seamless"
    frameborder="0">
</iframe>



<iframe src="/assets/img/bokeh/Revenue Bar Chart.html"   
    sandbox="allow-same-origin allow-scripts"
    width="100%"
    height="500"
    scrolling="no"
    seamless="seamless"
    frameborder="0">
</iframe>



<iframe src="/assets/img/bokeh/revenues vs rank scatter.html"   
    sandbox="allow-same-origin allow-scripts"
    width="100%"
    height="500"
    scrolling="no"
    seamless="seamless"
    frameborder="0">
</iframe>




[1]:	https://www.kaggle.com/datasets/darinhawley/fortune-500-companies-19552021/