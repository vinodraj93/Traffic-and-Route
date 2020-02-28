
## Introduction
### Abstract

We are focusing on building a dataset with route information and incidents (constructions, collisions, etc.) happening within that routes. In order to do that we need a route data and incidents data to find the time taken to travel between two routes.


## Data Acquisition & Pre-Processing

1. Get the Route, Incidents, latitude/longitude details within a Source and Destination from MapQuest API
2. Cleaning the Route details between source and destination and extracting only the following variables¶
    * Direction
    * Distance in between flags (& Cumulative)
    * Current Time and Time to reach (& Cumulative)
    * Lat & Long of each flag (bounding box)
3. Formatting Date-time and Calculating time from current time¶
4. Calculaing the time to reach from current time in between flags until the destination
5. Cleaning the Incidents happening between the source and destination¶
    * Incident type
    * Start time and end time of incidents (DateTime formats)
    * Delay Time
    * Lat and Long
6. Merging Routes and Incidents based on Date-time, Latitude and Longitude¶
    * Incident’s Lat and Long in line/in direction with the Route’s Lat and Long
    * Route’s Current Date-time is in between the Incident’s Start and End DateTime
    * Add Delay time to Time to reach if the incident aligns Route
7. Exporting the Dataset as CSV

pv = Python Variable, cv = Column Variable pyfn = Python Function 

### Data Acquisition <br>
get_routes_incidents (pyfn) | function to extract API <br>
Source (pv)| Address of starting point <br>
Destination (pv)| Address of end point <br>
routeURL (pv)| URL of Route API <br>
route1 (pv)| Extracting Route data using requests library <br>
route2 (pv)| Extracting Route JSON <br>
boundingbox (pv)| Latitude of Source, Longitude of Source, Latitude of Destination , Longitude of Destination <br>
incURL (pv)| URL of Incident API <br>
inciden (pv)| Extracting Incidents data using requests library <br>
incidents (pv)| Extracting Incidents JSON <br>
boundingbox (pv)| Latitude of Source, Longitude of Source, Latitude of Destination , Longitude of Destination <br>


### Getting the results using above function between a Source and Destination<br>
route (pv)| extracted JSON of Route details<br>
Incidents (pv)| extracted JSON of Incidents<br>


### Data Pre-Processing<br>
Routeddict (pv)| A dictionary of cleaned route details with directionName, distance, formattedTime, streets, narrative, time, startPoint.<br>
directionName (cv)| Directions North, South, East, West<br>
distance (cv)| distance in miles between streets<br>
formattedTime (cv)| formatting time in seconds too hh.mm.ss<br>
streets (cv)| flags between source and destination<br>
narrative (cv)| directions guide in text<br>
time (cv)| time taken to reach each flag in seconds<br>
startPoint (cv)| Latitude and Longitude of each flags<br>


### Formatting Datetime and Calculation time from current time<br>
now (pv)| getting the current date and time<br>
now1 (pv)| converting now to time delta to get hours, minutes and seconds separately<br>
Currenttime (cv)| Time delta after adding formatted time to now1<br>
cumsum (pyfn)| function to calculate cumulative sum<br>
dist (pv)| dictionary of cumulative distance<br>
time (pv)| dictionary of cumulative time<br>

### Incidents happening between the source and destination<br>
Incidentdict (pv)| Dictionary of cleaned incidents<br>
endTime (pv)| Datetime delta for incident’s end time<br>
startTime (pv)| Datetime delta for incident’s start time<br>
shortDesc (cv)| Short description of incident<br>
delayFromTypical(cv)| Delay time<br>
type(cv)|type of incident<br>


### Merging Routes and Incidents based on Datetime, Latitude and Longitude<br>
findcord (pyfn)| function to check if the incident’s latitude and longitude falls between source’s lat & long and destination’s lat&long<br>
xycords (pv)| lat & long of flags (start point)<br>
tstcords (pv)| lat & long of incidents<br>
source (pv)| Source Address<br>
destination (pv)| destination Adress<br>
Startat (pv)| Starting time from source<br>
ttor (pv)| Time to reach destination
td (pv)| Total distance between source and destination <br>
nar (pv)| Directions guid in text<br>
dfor (pv)| distance between flags<br>
Rat (pv)| Time to reach flags<br>
st (pv)| streets(flags)<br>
inshd (pv)| Short description of incident <br>
dl (pv)| Delay time<br>
toin (pv)| Type of incident (1 = ’Construction' , 2 = ’Event’ ,3= 'Congestion'  ,4= ’Accident’)<br>
inclat (pv)| Incident’s Latitude<br>
inclong (pv)| Incident’s Longitude<br>
toin1 (pv)| Hard coded toin <br>
Dataset (pv)| final dataset <br>



### Who will be using the Dataset?
Direction and routing Applications like Google Maps, Waze, etc. will be interested in this dataset.
They can utilize this dataset to build an optimized route between two places.
Personaly application can also be developed using this dataset.


### Limitations
Incident’s latitude and longitude may not align within the route between source and destination.
Null values in Streets/Highway information.