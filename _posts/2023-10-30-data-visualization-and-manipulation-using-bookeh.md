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
  path: /assets/img/sp500.jpg
  alt: Responsive rendering of Chirpy theme on multiple devices.
---

# Data Visualization and manipulation using Bookeh

Bokeh is a Python library for creating web apps with interactive data visualizations.

The following code shows different implementation of the bookeh ot crate vsiaialiton for a dataset of the Fortune 500 Companies 1955-2021.

Here are some intro to the dataset from [Kaggle][1]:

## **About Dataset**

Since 1955, Fortune magazine has published the top 500 companies in the US based on annual revenue. This list shows all 500 for each year. Note: 2015 is missing revenue figures.

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
<p>The following dataset contains the revenues of the largest US companies in the S&P 500 over the past few years. In this first iteration, we are focusing only on the top companies. Our objective is to</p>


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


### Output

\<iframe src="/assets/img/bokeh/Revenue Sample.html"  
    sandbox="allow-same-origin allow-scripts"
	width="100%"
	height="750"
	scrolling="no"
	seamless="seamless"
	frameborder="0">
</iframe>

## Basic Line Plot of Revenues over Years


	p1 = figure(title="Revenues Over Years", x_axis_label="Year", y_axis_label="Revenue")
	
	# Plot a line for each company
	for company in data['Name'].unique():
	    company_data = data[data['Name'] == company]
	    p1.line(company_data['Year'], company_data['Revenue'], legend_label=company, line_width=2)
	
	p1.legend.location = "right"
	p1.legend.click_policy = "mute"
	p1.legend.background_fill_alpha = 0.5
	
	
	output_file("revenue_by_company_over_years.html")
	show(p1)


### Output

\<iframe src="/assets/img/bokeh/Revenue by Company.html"  
    sandbox="allow-same-origin allow-scripts"
	width="100%"
	height="650"
	scrolling="yes"
	seamless="seamless"
	frameborder="0">
</iframe>

## 2. Bar Chart of Companies' Revenues for 2021

	# Filter the data for the year 2021
	top_companies_2021 = data[data['Year'] == 2021]
	
	# Sort the data by revenue and select the top 10 companies
	top_N_companies_2021 = top_companies_2021.sort_values(by="Revenue", ascending=False).head(10)
	
	# Create a data source for the top 10 companies
	source = ColumnDataSource(data=dict(names=top_N_companies_2021['Name'], revenues=top_N_companies_2021['Revenue']))
	
	# Create the bar chart
	p2 = figure(x_range=top_N_companies_2021['Name'], title="Top 10 Companies' Revenues for 2021", 
	            x_axis_label="Company", y_axis_label="Revenue", 
	            height=500, width=800, tools="")
	p2.vbar(x='names', top='revenues', width=0.8, color="navy", source=source)
	p2.xaxis.major_label_orientation = "vertical"
	p2.xaxis.major_label_text_font_size = "10pt"
	p2.ygrid.grid_line_alpha = 0.5
	p2.xgrid.grid_line_color = None
	
	output_file("revenue_top_10_bar_chart.html")
	show(p2)


### Output

\<iframe src="/assets/img/bokeh/revenue\_top\_10\_bar\_chart.html"  
    sandbox="allow-same-origin allow-scripts"
	width="100%"
	height="500"
	scrolling="no"
	seamless="seamless"
	frameborder="0">
</iframe>

## Scatter Plot for Revenues vs. Rank for 2021

	# Convert the columns to numeric types
	data['Rank'] = pd.to_numeric(data['Rank'], errors='coerce')
	data['Revenue'] = pd.to_numeric(data['Revenue'], errors='coerce')
	
	# Determine the range for Rank and Revenue
	rank_range = (data['Rank'][data['Year'] == 2021].min(), data['Rank'][data['Year'] == 2021].max())
	revenue_range = (data['Revenue'][data['Year'] == 2021].min(), data['Revenue'][data['Year'] == 2021].max())
	
	# Adjust padding for better visualization
	padding_rank = (rank_range[1] - rank_range[0]) * 0.05
	padding_revenue = (revenue_range[1] - revenue_range[0]) * 0.05
	
	rank_range = (rank_range[0] - padding_rank, rank_range[1] + padding_rank)
	revenue_range = (revenue_range[0] - padding_revenue, revenue_range[1] + padding_revenue)
	
	# Create the plot with the specified x_range and y_range
	p3 = figure(title="Revenues vs. Rank for 2021", 
	            x_axis_label="Rank", 
	            y_axis_label="Revenue", 
	            tools=[hover, "pan,reset,wheel_zoom"],
	            x_range=rank_range,
	            y_range=revenue_range)
	
	p3.circle('Rank', 'Revenue', size=10, color="navy", alpha=0.5, source=source)
	
	output_file("revenues_vs_rank_scatter.html")
	show(p3)



### Output

\<iframe src="/assets/img/bokeh/revenues\_vs\_rank\_scatter.html"  
    sandbox="allow-same-origin allow-scripts"
	width="100%"
	height="650"
	scrolling="no"
	seamless="seamless"
	frameborder="0">
</iframe>


 

[1]:	https://www.kaggle.com/datasets/darinhawley/fortune-500-companies-19552021/