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


### Data Information

All the data are fetched from [Lufdaten](http://archive.luftdaten.info/). Below are the sensor list contains when they are added to database.

|**Sensors**     |  **Start Date**  |  **Ending Dates** (End Date, Start Date, End Date ...)  |
|---|---|---|
|bme280 | 2017-03-02| -  |
|bmp180 | 2017-02-08|  - |
|bmp280 | 2017-11-26| -  |
|dht22 |2016-01-01  |  - |
|ds18b20 |2018-05-01   |  - |
|hpm| 2017-11-25  |  - |
|htu21d | 2017-11-26| [2018-01,2018-05, -]  |
|pms3003 |  2017-10-08| [2017-11, 2017-12, -]  |
|pms5003 |  2018-03-09|  - |
|pms7003 |  2017-10-08| -  |
|ppd42ns|2015-10-01| - |
|sds011 | 2016-07-14| -  |

### Sample Columns from each Data Set

sensor_id|sensor_type|location|lat|lon|timestamp|pressure|altitude|pressure_sealevel|temperature|humidity|
|---|---|---|---|---|---|---|---|---|---|---|
113|BME280|3093|48.809|9.181|2018-12-31T00:00:07|99561.02|-|-|5.07|100.00
615|BMP180|279|48.769|9.185|2018-12-31T00:00:06|99495.00|-|-|8.00|
12309|BMP280|6217|48.150|17.142|2018-12-31T00:02:04|100858.00|-|-|6.87|

sensor_id|sensor_type|location|lat|lon|timestamp|temperature|humidity|
|---|---|---|---|---|---|---|---|
93|DHT22|42|48.800|9.003|2018-12-31T00:02:02|5.00|95.10|
584|HTU21D|278|54.085|12.127|2018-12-31T00:04:15|6.01|91.74|

sensor_id|sensor_type|location|lat|lon|timestamp|temperature|
|---|---|---|---|---|---|---|
8020|DS18B20|4054|52.666|4.771|2018-12-31T00:00:10|9.06|

sensor_id|sensor_type|location|lat|lon|timestamp|P1|P2|
|---|---|---|---|---|---|---|---|
10975|HPM|5541|50.797|4.359|2018-12-31T00:02:17|548|83|

sensor_id|sensor_type|location|lat|lon|timestamp|P1|P2|P0|
|---|---|---|---|---|---|---|---|---|
397|PMS3003|187|24.074|120.340|2018-12-31T00:00:09|3.00|3.00|-|
10689|PMS5003|5394|27.221|78.010|2018-12-31T00:02:00|459.00|356.25|163.50|
8761|PMS7003|4417|42.139|24.794|2018-12-31T00:44:05|55.00|42.60|25.80|

sensor_id|sensor_type|location|lat|lon|timestamp|P1|durP1|ratioP1|P2|durP2|ratioP2|
|---|---|---|---|---|---|---|---|---|---|---|---|
48|PPD42NS|19|48.721|9.209|2018-12-31T00:33:48|8471.07|3880318|12.93|1013.84|588197|1.96|
92|SDS011|42|48.800|9.003|2018-12-31T00:02:01|13.22|-|-|11.93|-|-|

[luftdaten.info]: https://luftdaten.info
[luftdaten licensing]: https://archive.luftdaten.info/00disclamer.md
[ODbL]: https://opendatacommons.org/licenses/odbl/1.0/
