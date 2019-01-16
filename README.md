# Air quality data

A toolkit to retrieve, analyze and visualize data from a variety of air quality
sensors from luftdaten.info.

The scripts include tools to  
* wrap the APIs of the [luftdaten.info] project
* represent sensors of those different providers as objects with a unified
  interface to make it easy to interact with them  
* retrieve sensor measurement data through API calls  
* cache those data  
* clean and combine the data  
* describe measurements statistically - individual sensors or groups to
  compare  
* plot measurement time series  
* find sensors that are geographically close to a point of interest or to other
  sensors  

See the `Air Quality Analysis` notebook for more details.

## Installation
To install, download the repository to your project folder and use `import airquality` 

A Python 3.5+ environment and several Python packages are required. 
See requirements.txt and
install_requirements.sh in this repository.


Data made available by the luftdaten.info project are [licensed][luftdaten
licensing] under the [Open Database License][ODbL].


## Files and Functions
`utils.py` --  Contains  useful functions for data analysis.
 * `BaseSensor Class` -- Template class. Contains basic structure of the sensors. This class
 is planned to use with other open similar data sources.
 * `BaseSensor.get_metadata` --  Get sensor metadata and current measurements if attached. Override later.
 * `BaseSensor.get_measurements` -- Get measurement data. Override later.
 * `BaseSensor.clean_measurements_with_particles` -- Clean measurement data. Override later.
 * `BaseSensor.intervals` -- Histogram of times between measurements.
 * `BaseSensor.get_hourly_means` -- Calculate hourly means from measurement data.
 * `BaseSensor.plot_data` -- Plot times series data.
 * `BaseSensor.plot_measurements` -- Plot measurements as time series.
 * `BaseSensor.plot_hourly_means` -- Plot hourly means of measurements as time series .
 * `Call Rate Limiter Class` -- Rate limiter for API calls.
 * `read_json` -- Read a semi-structured JSON file into a flattened dataframe.
 * `retrieve` -- Get a resource file from cache or from a URL and parse it. Cache downloaded data.
 * `haversine` -- Calculate the great circle distance between two points on earth.
 * `label_coordinates` -- Combine a set of numeric coordinates into a string with hemisphere indicators.
 * `clear_cache` -- Remove the content of the cache directory if it exists.  
 
`luftdaten.py` --  Contains specific details about the Luftdaten sensors info.
* `Sensor Class` -- A sensor registered on luftdaten.info. Properties in addition to those of BaseSensor:
metadata_url: URL on luftdaten.info that provides the sensor's metadata and current measurements.
 * `Sensor.get_metadata` --  Get sensor metadata and current measurements from cache or luftdaten.info API.
 * `Sensor.get_measurements` -- Get measurement data of the sensor in a given period.
Data are read from cache if available, or downloaded from luftdaten.info and saved to cache as retrieved, and then
cleaned for self.measurements. If the instance already has data associated with it, calling this method replaces them.
 * `Sensor.clean_measurements_with_particles` -- Remove invalid measurements.
This function only cleans data with PM10 and PM2.5 values. Having several consecutive values under 1 µg/m³ indicates a
problem with the sensor -> discard those values. Values of 999.9 µg/m³ (PM2.5) and 1999.9 µg/m³ (PM10) also
indicate a problem.
 * `search_proximity` -- Find sensors within given radius from a location. 
 Default values are the approximate center and radius of Stuttgart
 * `evaluate_near_sensors` -- Create Sensor instances for all sensors of sensor_type near a
location and get their measurement data. Coordinates and radius default to Stuttgart.

`__init__.py`
* `compare_sensor_data` -- Compare the measurements of a group of sensors. 
Values are plotted and returned


[luftdaten.info]: https://luftdaten.info
[luftdaten licensing]: https://archive.luftdaten.info/00disclamer.md
[ODbL]: https://opendatacommons.org/licenses/odbl/1.0/
