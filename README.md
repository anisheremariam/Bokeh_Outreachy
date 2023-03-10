# FUNDAMENTALS OF DATA VISUALIZATION IN BOKEH
I know you are thinking "Why Bokeh?". Well relax, I will get there. First, let's start with the "What is?".

### What is Bokeh?
Bokeh is a Python library for creating interactive visualizations for modern web browsers. It helps you build beautiful graphics, ranging from simple plots to complex dashboards with streaming datasets. With Bokeh, you can create JavaScript-powered visualizations without writing any Script. It provides you output in various medium like html or notebook. Bokeh has flexibility for applying interaction, layouts and different styling option to visualization. Bokeh is a very powerful library for visualization and that is **WHY** we are talking about the fundamentals of data visualization in bokeh.

The first step to data visualization is; Preparation of data which include Data Gathering and Wrangling. For this visualization, we will be working with the New York City Taxi and Limousine Trip Record Data; https://www.nyc.gov/site/tlc/about/tlc-trip-record-data.page.

The major concept of Bokeh is that graphs are built up one layer at a time. The basic fundamental use of Bokeh can be summarized into six (6) simple stages:
* Import Libraries;
* Prepare Data;
* Select an output mode (HTML or Notebook);
* Create a blank canvas;
* Insert desired glyph into canvas;
* Show Visualization.
#

The SCATTER PLOT representation: This can be achieved by using the ```.scatter``` or ```.circle/diamond``` and other shapes as markers.
(check 'https://docs.bokeh.org/en/dev-3.1/docs/user_guide/basic/scatters.html' for available markers)
```
# import libraries
import pandas as pd  #Data Preparation
import pyarrow.parquet as pq #To view the paraquet format
from bokeh.io import output_file, output_notebook #for Output
from bokeh.plotting import figure, show #for charts

# rendering mode
output_file('output_file_test.html', title='Empty Bokeh Figure') # for a static html
#output_notebook() # for inline jupyter notebook

# create a blank figure canvas
my_viz = figure(title = "Relationship Between the Time Taken and Price of Trip",
                height = 500, width = 800,
               x_axis_label = "Time Taken to complete a Trip (min)", y_axis_label = "Price of Trip (USD)",
               x_range = (1, 146), y_range = (1,1250))

# insert glyph
my_viz.scatter(x = trips["trip_distance"], y = trips["total_amount"])

# show visualization
show(my_viz)
```
<img src="https://github.com/anisheremariam/Bokeh_Outreachy/blob/main/bokeh_scatterplot.png" alt="Scatter Plot" title="Scatter Plot">

#

The DOUBLE PLOT representation: One canva can be used to represent more than one plot. Here is an example with two basic plots (the line and bar plot) on a single canva.
```
# import libraries
import pandas as pd   # for data preparation
import pyarrow.parquet as pq    # to view the paraquet format
from bokeh.io import output_file, output_notebook   # for Output
from bokeh.plotting import figure, show  # for charts
from bokeh.io import curdoc   # to apply theme

# rendering mode
output_file('output_file_test.html', title='Empty Bokeh Figure') # for a static html
#output_notebook() # for inline jupyter notebook

# create a blank figure canvas
my_viz = figure(title = "Daily Tips and Tolls Amount",
                height = 500, width = 800,
               x_axis_label = "November 2022", y_axis_label = "Amount (USD)")
               
# change theme to dark
curdoc().theme = "dark_minimal"

# insert line_glyph
my_viz.line(x = day_trip.index, y = day_trip["tip_amount"],
           color = "orange", line_width = 3, legend_label = "Tip Amount")

# insert bar_glyph
my_viz.vbar(x = day_trip.index, top = day_trip["tolls_amount"],
           color = "green", width = 0.75, legend_label = "Tolls Amount")

# arrange legend
my_viz.legend.location = 'top_left'

# show visualization
show(my_viz)
```
<img src="https://github.com/anisheremariam/Bokeh_Outreachy/blob/main/bokeh_doubleplot.png" alt="Double Plot" title="Double Plot">
