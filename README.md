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

You will start importing the libraries, the OpenWeatherMap API key and Import citipy to determine the cities based on latitude and longitude.

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
Then, you should generate a Random the Cities List by Using the citipy Library. 
CITATION.cff 
message: "If you use this code, please cite it as below."
authors:
  - name: Bootcamp Toronto University
    orcid: [https://git.bootcampcontent.com/]
title: "Challenge 6"


```
# Empty list for holding the latitude and longitude combinations
lat_lngs = []

# Empty list for holding the cities names
cities = []

# Range of latitudes and longitudes
lat_range = (-90, 90)
lng_range = (-180, 180)

# Create a set of random lat and lng combinations
lats = np.random.uniform(lat_range[0], lat_range[1], size=1500)
lngs = np.random.uniform(lng_range[0], lng_range[1], size=1500)
lat_lngs = zip(lats, lngs)

# Identify nearest city for each lat, lng combination
for lat_lng in lat_lngs:
    city = citipy.nearest_city(lat_lng[0], lat_lng[1]).city_name
    
    # If the city is unique, then add it to a our cities list
    if city not in cities:
        cities.append(city)

# Print the city count to confirm sufficient count
print(f"Number of cities in the list: {len(cities)}")
# Empty list for holding the latitude and longitude combinations
lat_lngs = []

# Empty list for holding the cities names
cities = []

# Range of latitudes and longitudes
lat_range = (-90, 90)
lng_range = (-180, 180)

# Create a set of random lat and lng combinations
lats = np.random.uniform(lat_range[0], lat_range[1], size=1500)
lngs = np.random.uniform(lng_range[0], lng_range[1], size=1500)
lat_lngs = zip(lats, lngs)

# Identify nearest city for each lat, lng combination
for lat_lng in lat_lngs:
    city = citipy.nearest_city(lat_lng[0], lat_lng[1]).city_name
    
    # If the city is unique, then add it to a our cities list
    if city not in cities:
        cities.append(city)

# Print the city count to confirm sufficient count
print(f"Number of cities in the list: {len(cities)}")
```
Use the OpenWeatherMap API to retrieve weather data from the cities list generated in the started code.
You should set the API Base URL and units of measurement that you can find in the OpenWeatherMap API documents  [https://openweathermap.org/current]. After that create an queary URL.
```
url = "http://api.openweathermap.org/data/2.5/weather?"
units= "metric"
query_url = f"{url}appid={weather_api_key}&units={units}&q="
```
Then
```
# Define an empty list to fetch the weather data for each city
city_data = []

# Print to logger
print("Beginning Data Retrieval     ")
print("-----------------------------")

# Create counters
record_count = 1
set_count = 1

# Loop through all the cities in our list to fetch weather data
for i, city in enumerate(cities):
        
    # Group cities in sets of 50 for logging purposes
    if (i % 50 == 0 and i >= 50):
        set_count += 1
        record_count = 0

    # Create endpoint URL with each city
    city_url = query_url + city
    
    # Log the url, record, and set numbers
    print("Processing Record %s of Set %s | %s" % (record_count, set_count, city))

    # Add 1 to the record count
    record_count += 1
```


