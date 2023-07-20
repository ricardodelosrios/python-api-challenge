# python-api-challenge
## Background
Data's true power is its ability to definitively answer questions. So, let's take what you've learned about Python requests, APIs, and JSON traversals to answer a fundamental question: "What is the weather like as we approach the equator?"

Now, we know what you may be thinking: “That’s obvious. It gets hotter.” But, if pressed for more information, how would you prove that?

## Installation

To install:

* To install these libraries, activate your virtual environment, then run:
`conda install -c pyviz hvplot geoviews`
`pip install census`
`pip install citipy` 

* To make sure these libraries installed properly, run:
`conda list hvplot`
`conda list geoviews`

*You will use the OpenWeatherMap API [https://openweathermap.org/api] and Geoapify API [https://www.geoapify.com/]
Create a file call `api_keys.py` and paste your OpenWeatherMap API and Geoapify API.

## Getting Started

This activity is broken down into two, WeatherPy and VacationPy.

### WeatherPy

#### Important things to understand and run the script

It will start importing the libraries and modules:

* It will set up the `matplotlib` library, With `matplotlib.pyplot`, it will create for this activity scatter plots, but if you need it will create a different kinds of graphs like line plots, bar plots, histograms, and much more.
* It will set up the `pandas` library.
* configurará un paquete `numpy` y realizará un conjunto de combinaciones aleatorias de latitud y longitud para encontrar las ciudades.
* It will set up `requests` library , with this library it will interact with openweather, send a requests, and receive responses.
* The `time` module is used to work with time values, delays, and timestamps.
* The `linregress` function is a part of the `scipy.stats` module and it will be used to calculate a linear regression model according to weather parameters.
* it will import the `datetime` module in Python. This module will allow you to work with dates and times in Python.
* It will import an API key named `weather_api_key` from a file named `api_keys`. This is going to be important to authenticate your requests to an external API (OpenWeather).
* It will import `citipy` to determine the cities based on latitude and longitude.

```
import matplotlib.pyplot as plt
import pandas as pd
import numpy as np
import requests
import time
from scipy.stats import linregress
from datetime import datetime

from api_keys import weather_api_key

from citipy import citipy
```

Use the OpenWeatherMap API to retrieve weather data from the cities list generated in the started code.
it should set the API Base URL and units of measurement that you can find in the OpenWeatherMap API documents  [https://openweathermap.org/current]. After that create a queary URL.
```
url = "http://api.openweathermap.org/data/2.5/weather?"
units= "metric"
query_url = f"{url}appid={weather_api_key}&units={units}&q="
```
To analyze latitude, longitude, maximum temperature, humidity, cloud cover, wind speed, country, and date, it must have the Weather fields found in the OpenWeatherMap API documentation. [https://openweathermap.org/current].

*`coord.lat` Latitude of the location
*`coord.lon` Longitude of the location
*`main.temp` Temperature. Unit Default: Kelvin, `Metric: Celsius`, Imperial: Fahrenheit.
*`main.humidity` Humidity, %
* `clouds.all` Cloudiness, %
* `wind.speed`
* `sys.country` Country code (GB, JP etc.)
* `dt` Time of data calculation

This line of code `datetime.fromtimestamp(city_weather['dt']).strftime('%y-%m-%d')` helps you extract the date from the weather data and format it in a way that is easy to display or work. For example, for this activity, it use the date in some of the graphs.

```
city_lat = city_weather['coord']['lat']
        city_lng = city_weather["coord"]["lon"] 
        city_max_temp = city_weather['main']['temp']
        city_humidity = city_weather['main']['humidity']
        city_clouds = city_weather['clouds']['all']
        city_wind = city_weather['wind']['speed']
        city_country = city_weather['sys']['country']
        city_date = datetime.fromtimestamp(city_weather['dt']).strftime('%y-%m-%d')
```
it will create a data set that it will find in a folder called `output_data` and the file will be called `cities.csv`.
```
 Export the City_Data into a csv
city_data_df.to_csv("output_data/cities.csv", index_label="City_ID")

# Read saved data
city_data_df = pd.read_csv("output_data/cities.csv", index_col="City_ID")

# Display sample data
city_data_df.head()

```
it will create 4 Scatter Plots that it will find in a folder called `output_data` and the images will be called `Fig1.png`, `Fig2.png`, `Fig3.png`, and `Fig4.png`.
* `Fig1.png` scatter plot for latitude vs. temperature.
* `Fig2.png` scatter plot for Latitude Vs. Humidity.
* `Fig3.png` scatter plot for Latitude Vs. Cloudiness.
* `Fig4.png` scatter plot for Latitude vs. Wind Speed Plot.

For the last step, it will create a function to create linear regression graphs that can be used multiple times in this program, preventing code repetition.
```
# Define a function to create Linear Regression plots
def create_lin_reg(df:pd.DataFrame, weather:str, xpos:int, ypos:int)->None:

    # Create base scatter plot
    df.plot.scatter(x='Lat', y=weather, s=40)
 
    # Calculate linear regression
    slope, intercept, rvalue, pvalue, stderr = linregress(df['Lat'], df[weather])
    regress = df['Lat'] * slope + intercept
    line_eq = "y = " + str(round(slope,2)) + "x +" + str(round(intercept,2))

    # Plot linear regression line and equation
    plt.plot(df['Lat'], regress, color='red')
    plt.annotate(line_eq, (xpos, ypos), fontsize=15, color='red')

    # Add other graph properties
    plt.title(f"The r-value is: {rvalue}", loc='left')
    plt.xlabel('Latitude')

    # Display results
    plt.show()
```
Then it will create a DataFrame with the Northern Hemisphere data and with the Southern Hemisphere.

After that, it will create 8 Linear Regression Plots, 4 for the Northern Hemisphere and 4 for the Southern Hemisphere taking into account the following variables:

*Temperature vs. Latitude
*Humidity vs. Latitude
*Cloudiness vs. Latitude
*Wind Speed vs. Latitude

### VacationPy

#### Important things to understand and run the script

* It will set up the `pandas` library.
* It will import the `hvplot` extension for Pandas in Python. `hvplot` is a library that allows you to create interactive visualizations directly from Pandas DataFrames.
* It will set up `requests` library , with this library it will interact with geoapify, send a requests, and receive responses.
* The `json` module is from Python standard library and provides functions for working with data in JSON (JavaScript Object Notation) format.
* It will import an API key named `geoapify_key` from a file named `api_keys`. This is going to be important to authenticate your requests to an external API (geoapify).

  ```
  # Dependencies and Setup
  import hvplot.pandas
  import pandas as pd
  import requests
  import json

  # Import API key
  from api_keys import geoapify_key
  ```
The first step is to load the file called `cities.csv`  that is in the folder called `output_data`.
  ```
  # Load the CSV file created in Part 1 into a Pandas DataFrame
  city_data_df = pd.read_csv("output_data/cities.csv")

  # Display sample data
  city_data_df.head()
  ```
Then, it will create a map that displays a point for every city in the city_data_df DataFrame. To display this graph it will be use a `hvplot` library and it is going to use the following parametres:

* It will call the latitude `Lng` and longitude `Lat` variables from the DataFrame
* To display this plot a `hvplot` library will be used and the following parameters will be used.
* It will call the latitude `Lat` and longitude `Lng` variables of the DataFrame.
* `geo=True` will be an argument that could be a method to enable geodata.
* `Tiles='EsriNatGeo'` is a map style that is inspired by traditional National Geographic maps. 
* The frame size is going to be determined according to the `frame_width` and  `frame_height` parameters.
* `size='Humidity` means that each point in the map is according to the size of the values in the variable 'Humidity'.
* `color='City'` means that each city will have a different color.
* `scale=0.8` it will be the scale of each point in the map.

After that, it will create a new DataFrame to find an ideal weather conditions. To this activity it consider that the best weather has the following criteria:

* temperature =  >= 18 & <= 24
* wind_speed =  < 4
* cloudiness_new = >= 0 &  <= 20
* humidity_new = >= 30 &  <= 60
 ```

temperature = (city_data_df['Max Temp'] >= 18) & (city_data_df['Max Temp'] <= 24)
wind_speed = (city_data_df['Wind Speed'] < 4)
cloudiness_new = (city_data_df['Cloudiness'] >= 0) & (city_data_df['Cloudiness'] <= 20)
humidity_new = (city_data_df['Humidity'] >= 30) & (city_data_df['Humidity'] <= 60)
ideal = city_data_df.loc[temperature & wind_speed & cloudiness_new & humidity_new]

# Drop any rows with null values
ideal.dropna(how='any')

# Display sample data
print(len(ideal))
ideal.head()
```

Then, it will use `Geoapify` API to find one hotel located within 10.000 meters of each city that is in the new DataFrame. It is important to understand the following parameters:

it must use the categories `accommodation.hotel` found in the Geoapify API documentation. [https://apidocs.geoapify.com/docs/places/#about].
it will use `limit=1` because it will just need 1 hotel per location.
From the file apiKey it will get the `geoapify_key` 
it should set the API Base URL that you can find in the Geoapify API documentation [https://api.geoapify.com/v2/places?PARAMS]. 









