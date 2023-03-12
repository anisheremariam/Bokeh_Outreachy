## Author: Anishere Mariam Adeola
## Date: March 2023
# FUNDAMENTALS OF DATA VISUALIZATION IN BOKEH
I know you are thinking "Why Bokeh?". Well relax, I will get there. First, let's start with "What is Bokeh?".

Bokeh is a Python library for creating interactive visualizations for modern web browsers. It helps you build beautiful graphics, ranging from simple plots to complex dashboards with streaming datasets. With Bokeh, you can create JavaScript-powered visualizations without writing any Script. It provides you output in various medium like html or notebook. Bokeh has flexibility for applying interaction, layouts and different styling option to visualization. Bokeh is a very powerful library for visualization and that is **WHY** we are talking about the fundamentals of data visualization in bokeh.

The first step to data visualization is; Preparation of data which include Data Gathering and Wrangling. For the purpose of this series, I will be working with the `Yellow_Trip_Records for November 2022` which can be found on the [New York City Taxi and Limousine Commission website](https://www.nyc.gov/site/tlc/about/tlc-trip-record-data.page).

The major concept of Bokeh is that graphs are built up one layer at a time. The basic fundamental use of Bokeh can be summarized into six (6) simple stages:
* Import Libraries;
* Prepare Data;
* Select an output mode (HTML or Notebook);
* Create a blank canvas;
* Insert desired glyph into canvas;
* Show Visualization.

Run this code to read file in parquet format:
```
import pandas as pd
import pyarrow.parquet as pq
trips = pq.read_table('yellow_tripdata_2022-11.parquet')
trips = trips.to_pandas()
print("Shape: ", trips.shape)
trips.head()
```
Run this code to read file in csv format:
```
import pandas as pd
trips = pd.read_csv('yellow_tripdata_2022-11.csv')
print("Shape: ", trips.shape)
trips.head()
```
Check [here](https://pandas.pydata.org/docs/user_guide/io.html) to find out how to read other formats.

## BASIC VISUALIZATIONS IN BOKEH
The SCATTER PLOT representation: This can be achieved by using the ```.scatter``` or ```.circle/diamond``` and other shapes as markers.
(check [here](https://docs.bokeh.org/en/dev-3.1/docs/user_guide/basic/scatters.html) for available markers)
```
# import libraries
import pandas as pd  #Data Preparation
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

#

The ROD PLOT representation: The scatter plot can be represented with several shapes like the circle, rectangle squares, etc. The rod plot is a rectangular scatter plot tilted by an angle (60 deg). You can think of it as a "Fancy Scatter Plot".
```
# import libraries
import pandas as pd  #Data Preparation
from bokeh.io import output_file, output_notebook #for Output
from bokeh.plotting import figure, show #for charts
from math import pi

# prepare data
pick_up = trips.groupby(["PULocationID"])[["tip_amount"]].sum()

# rendering mode
output_notebook()

# create plot
p = figure(y_range = (1, 200),width=700, height=500)

# create circle renderer with color mapper
p.rect(x=pick_up.index, y=pick_up["tip_amount"], width=0.2, height=40, color="magenta",
       angle=pi/3, height_units="screen")

show(p)
```
<img src="https://github.com/anisheremariam/MyProjects/blob/main/bokeh_magenta%20rodplot.png" alt="Rod Plot" title="Rod Plot">

#

The BOKEH SUBPLOTS: I am sure you are familiar with subplot function in Matplotlib but it is okay if you are not, you can catch up on the [Matplotlib Library](https://matplotlib.org/stable/gallery/subplots_axes_and_figures/subplots_demo.html). In Bokeh library, we can build a related interface like that of the subplot. 
```
# import libraries
import pandas as pd  #Data Preparation
from bokeh.io import output_file, output_notebook #for Output
from bokeh.plotting import figure, show #for charts
from bokeh.layouts import row

# prepare some data
pay_type = trips.groupby(["payment_type"])[["extra"]].sum()

# rendering mode
output_notebook()

# create three plots with one renderer each
v1 = figure(width=450, height=250, background_fill_color="white")
v1.circle(x=pay_type.index, y=pay_type["extra"], size=12, color="blue", alpha=0.8)

v2 = figure(width=450, height=250, background_fill_color="#fafafa")
v2.plus(x=pay_type.index, y=pay_type["extra"], size = 12, color="orange", alpha=1)

v3 = figure(width=450, height=250, background_fill_color="black")
v3.square(x=pay_type.index, y=pay_type["extra"], size=12, color="red", alpha=1)

# put the results in a row and show
show(row(v1,v2,v3))
```
<img src="https://github.com/anisheremariam/MyProjects/blob/main/bokeh_multiplot.PNG" alt="Bokeh SubPlot" title="Bokeh SubPlot">

#

The BAR PLOT representation: One canva can be used to represent more than one plot. Here is an example with two basic plots (the line and bar plot) on a single canva.
```
# import libraries
import pandas as pd   # for data preparation
from bokeh.io import output_file, output_notebook   # for Output
from bokeh.plotting import figure, show  # for charts
from bokeh.models import ColumnDataSource
from bokeh.models import HoverTool # for hover tooltip
from bokeh.palettes import Bright4

# prepare some data
tax = trips.groupby(["payment_type"])[["mta_tax"]].sum()
payment = ["Credit card", "Cash", "No charge", "Dispute"]
mta = tax["mta_tax"]

source = ColumnDataSource(data=dict(payment=payment, mta=mta, color=Bright4))

# rendering mode
output_notebook()

# create empty canva
p = figure(x_range = payment, height=350, title="Tax Payed Through Each Payment Type",
           toolbar_location=None, tools="hover")
           
# create hover tools with chart descriptions
hover = p.select(dict(type=HoverTool))
hover.tooltips = [("Series", "Tax"),("Payment", "$x"),("Tax", "$y")]

# insert bar glyph
p.vbar(x='payment', top='mta', width=0.9, color='color', legend_field="payment", source=source)

# improve aesthatics
p.xgrid.grid_line_color = None
p.legend.orientation = "horizontal"
p.legend.location = "top_center"

#show visualization
show(p)
```
<img src="https://github.com/anisheremariam/MyProjects/blob/main/bokeh_bar.PNG" alt="Bar Plot" title="Bar Plot">
