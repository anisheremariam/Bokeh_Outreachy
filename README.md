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

## IMPORT LIBRARIES
Here, we import the neccesary libraries like the pandas or numpy for data preparation and bokeh for visualization.
```
#import libraries

#data preparation
import pandas as pd  

#to view the paraquet format
import pyarrow.parquet as pq 

#for output
from bokeh.io import output_file, output_notebook 

#for charts
from bokeh.plotting import figure, show 
```

## PREPARE DATA
This stage is basically the Data Wrangling process where we clean, transform, and analyse our data for visualization.

## SELECT AN OUTPUT MODE
With Bokeh, you cannot only view your output inline in a Jupyter Notebook but also on a static HTML with several toggle tools like the pan, zoom, etc.
```
#for static html
output_file('output_file_test.html', title='Empty Bokeh Figure')

#for inline Jupyter Notebook
output_notebook()
```
While rendereing multiple modes in a single project, it is pertinent to subsequently ```reset_output``` in order to clear the past mode from recurring.
```
# Import reset_output (only needed once) 
from bokeh.plotting import reset_output

# Use reset_output() between subsequent show() calls, as needed
reset_output()
```

## CREATE A BLANK CANVAS
Aesthetic is a very important aspect in visualisation. In order to send a clear and understanding message to the audience, your Data Visualization has to be appealing, direct and concise with perfect fonts size, color and background. Bokeh provides a layer to interact and customize our figure canvas with simple scripts.
```
# Create a blank figure canvas
my_viz = figure(title = "Relationship Between the Time Taken and Price of Trip",
                height = 500, width = 800,
               x_axis_label = "Time Taken to complete a Trip (min)", y_axis_label = "Price of Trip (USD)",
               x_range = (1, 146), y_range = (1,1250))
 ```
for more details on figure parameters, visit; https://docs.bokeh.org/en/latest/docs/user_guide.html.
 
## INSERT GLYPH
Glyph is a graphical shape or marker that is used to represent your data. In simple terms, Glyphs are Graphs. We will be working on several types of glyphs representation in this series. 

The first is the SCATTER PLOT representation. This can be achieved by using the ```.scatter``` or ```.circle/diamond``` and other shapes as markers
```
# Create a blank figure canvas
my_viz = figure(title = "Relationship Between the Time Taken and Price of Trip",
                height = 500, width = 800,
               x_axis_label = "Time Taken to complete a Trip (min)", y_axis_label = "Price of Trip (USD)",
               x_range = (1, 146), y_range = (1,1250))

# Insert glyph
my_viz.scatter(x = trips["trip_distance"], y = trips["total_amount"])

# Show visualization
show(my_viz)
```
<img src="https://github.com/anisheremariam/Anishere_Mariam/blob/main/bokeh_scatterplot.png" alt="Scatter Plot" title="Scatter Plot">
