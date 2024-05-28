# BikeAccess

## Overview
BikeAccess is a simple tool to run bicycle travel time calculations using R5 through R5py. R5 is a routing engine for multimodal (transit/bike/walk/car) networks developed to power Conveyal; R5py is a Python wrapper for the R5 routing analysis engine.

## Install software dependencies
### Install Java version 11
To interface with R5, r5py requires JDK 11 to function, if not already done, install JDK 11 following the [Java Installation Guide](https://docs.oracle.com/en/java/javase/11/install/overview-jdk-installation.html#GUID-8677A77F-231A-40F7-98B9-1FD0B48C346A).

### Install Python
Download and intall Python (this workflow is tested using Python 3.10 version. visit [Python download](https://www.python.org/downloads/) for more information.

### Install Python packages
Now python is installed, next is to install required python packages:

1. [R5py](https://github.com/r5py/r5py) is a Python wrapper for the R5 routing analysis engine.  
2. [pandas](https://pandas.pydata.org/) is a software library written for the Python programming language for data manipulation and analysis.  
3. [geopandas](https://geopandas.org/en/stable/) is an open source project to make working with geospatial data in python. (Note: Geopandas relies on several other dependencies, when using pip to install GeoPandas, you need to make sure that all dependencies are installed correctly. see https://geopandas.org/en/stable/getting_started/install.html for more information)  

Check the `requirements.txt` for all the required software dependencies. One could install the above packages individually via pip, or install at once with the `requirements.txt`:    

`pip install -r requirements.txt`  



## Prepare input datasets
R5py requires street network, destinations and origins as input datasets to perform travel time calculation. These input datasets should be prepared in specific formats:    
1. An OpenStreetMap network in `.pbf`  format     
2. The spatial data of origin/destination pairs in ESRI shapefile format, which can be used as origin/destination pairs in a travel time matrix calculation. The file should be a POINT sf object with WGS84 CRS containing 'id' and 'geometry' column.

## Calculate bicycle travel time Access
The tool contains a Python module and one configuration file:
- `cal_ttm_bike.py` is the analysis script that one would run for producing travel time access results. Running this analysis script requires an additional parameter, a configuration file `ttm_config.json`.  
- `ttm_config.json` is the configuration file that supports the `cal_ttm_bike.py`, it contains basic parameters for calculating bicycle travel time matrix.  

Run analysis scripts:  
`python cal_ttm_bike.py ttm_config.json`

Example configuration:  
```
{
    "java_path" : "/Library/Java/JavaVirtualMachines/openjdk-11.jdk",
    "data_path" : "data",
    "osm_filename" : "cook_county_bike_2022.osm.pbf",
    "origin_filename" : "CookCountyCensusBlockCentroids2020_WithLatLong_V02_PD_updated.shp",
    "destination_filename" : "TrailAccessPoints_WithLatLong_V06_PD_updated.shp",
    "max_lts" : 2,
    "max_trip_duration" : 30,
    "bike_speed" : 18.0,
    "walk_speed" : 5.0,
    "outputpath" : "blocks_to_trail_access_points_lts2.csv"
}
```
Configuration parameters:    
- `java_path` - A string character. The Java path. To know the Java path, run **/usr/libexec/java_home** if using Mac Terminal; or **echo %JAVA_HOME%** if using Windows Command Prompt.
- `data_path`  - A string character. Location of the input datasets. Default to `./data` in the parent directory
- `osm_filename` - A string character. Name of the OpenStreetMap file.
- `origin_filename` - A string character. Filename of the data that contains points to be used as origins. The file should be either a POINT sf object with WGS84 CRS, or a data.frame containing the columns id, lon and lat.  
- `destination_filename` - A string character. Filename of the data that contains points to be used as destinations. The file should be either a POINT sf object with WGS84 CRS, or a data.frame containing the columns id, lon and lat.  
- `max_lts` - An integer between 1 and 4. The maximum level of traffic stress that cyclists will tolerate. A value of 1 means cyclists will only travel through the quietest streets, while a value of 4 indicates cyclists can travel through any road.
- `max_trip_duration` - An integer. The maximum trip duration in minutes.   
- `bike_speed` - A numeric. Average cycling speed in km/h.  
- `walk_speed` - A numeric. Average walk speed in km/h.
- `ouputpath` - A string character. File location where the analysis results will be written to.
