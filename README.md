## Introduction
This guide recommends the capture, storage, and display of geometric representations.
<p align="center">
<img src="https://github.com/geological-survey-of-queensland/spatial-coordinate-handling/blob/master/images/spatial-coordinate-handling-overview.png"><br>
Figure 1: Lifecycle of a geometric representation</p>

## Background

 ### ISO 6709
* ISO 6709 _Standard representation of geographic point location by coordinates_ is the international standard for representation of latitude, longitude and altitude for geographic point locations.
* Under ISO 6709, a geographical point is specified by the following four items:
    * First horizontal coordinate (y), such as latitude (negative number south of equator and positive north of equator)
    * Second horizontal coordinate (x), such as longitude (negative values west of Prime Meridian and positive values east of Prime Meridian)
    * Vertical coordinate, i.e. height or depth (optional)
    * Identification of coordinate reference system (CRS) (optional)

### Coordinate reference system (CRS)
* A coordinate reference system or spatial reference system (SRS) is a coordinate-based local, regional or global system used to locate geographical entities.  
* Map projections try to portray the surface of the earth, or a portion of the earth, on a flat piece of paper or computer screen. Map projections try to transform the earth from its spherical shape (3D) to a planar shape (2D).  
* A coordinate reference system (CRS) then defines how the two-dimensional, projected map in your GIS relates to real places on the earth. The decision of which map projection and CRS to use depends on the regional extent of the area you want to work in, on the analysis you want to do, and often on the availability of data.

### GDA2020 CRS
* The datum Australia uses now is called the Geocentric Datum of Australia 1994, or GDA94.
* This grid of latitude and longitude coordinates moves with the drift of the continent, but satellite positioning systems base their coordinates on a framework that is fixed to the centre of the Earth.
* By 2020, Australia will have moved 1.8 metres north-east of where it was in 1994. The discrepancies between real-time, precise satellite positioning and GDA94 will be very noticeable.
* [GDA2020](https://www.icsm.gov.au/gda2020) defines a new datum 1.8 metres to the north-east of GDA94.
* Stage 2 of GDA2020 will establish a dynamic datum that will continually model Australia’s movement.

### SRID
* A Spatial Reference System Identifier (SRID) is a unique value used to unambiguously identify projected, unprojected, and local spatial coordinate system definitions.
* PostGIS uses the EPSG Geodetic Parameter Dataset.
* SRID examples are:
    * GDA2020 [EPSG:7844](https://epsg.io/7844)
    * WGS84 [EPSG:4326](https://epsg.io/4326)
    * GDA94 geographic coordinate system - EPSG:4283
    * GDA94/MGA Zones 49-56 (EPSG:28349 - EPSG:28356)
    * GDA2020/MGA Zones 49-56 (EPSG:7849 - EPSG:7856)

### PostGIS
*	PostGIS is a spatial database extension of PostgreSQL.
*	Spatial databases store and manipulate spatial objects using three features: spatial types, indexes, and functions.
* Geometries are stored in the PostGIS database in its native spatial format.
* Spatial data is converted on ingestion into the database (done by PostGIS).
* PostGIS spatial data can be output as WKT, WKB, GML, KML, GeoJSON, and SVG formats (done by PostGIS).
* PostGIS stores extended metadata about the spatial including the CRS (SRS).

### Coorindate transformation vs conversion
* Changing a coordinate on one datum (the source) to the corresponding coordinate on another datum (the target) involves a process called **coordinate transformation**.
* **Coordinate conversions** switch between coordinate reference systems on the same datum or reference frame, such as from a geographic coordinate (GDA2020 latitude and longitude) to a projected coordinate (MGA2020 Easting, Northing and Zone).

### GeoJSON
* [GeoJSON](https://tools.ietf.org/html/rfc7946) is an open standard format designed for representing simple geographical features, along with their non-spatial attributes. It is based on the JavaScript Object Notation (JSON).  
* GeoJSON is widely used in JavaScript web-mapping libraries, JSON-based document databases, and web APIs, and became the de facto standard for web mapping when Google Maps adopted it in 2005.
* GeoJSON uses the World Geodetic System 1984 (WGS 84) coordinate reference system, with longitude and latitude units of decimal degrees. 
* GSQ uses GeoJSON in its CKAN data catalogue to show the spatial representation of a dataset.
* The difference between GDA2020 and WGS84 coordinates was approximately 21 cm in January 2017, approximately 0 cm in January 2020, then 21 cm in 2023, and then continue to diverge by approximately 7cm per year. As GeoJSON is used as a representation of the geometry, not the precise geometry, this difference is not noticeable to the web user.

## How does industry submit geometries to GSQ?
GSQ receives co-ordinates from industry in the following formats:
* Latitude, longitde decimal
* Latitude, longitde degrees, minutes, seconds
* Eastings, northings, zone
* In binary format - e.g. a shapefile (ESRI .SHP format)
* In columns in Excel spreadsheets or CSV or .txt files

## Spatial coordinate workflow
<p align="center">
<img src="https://github.com/geological-survey-of-queensland/spatial-coordinate-handling/blob/master/images/spatial-coordinate-workflow.png" width="100%"><br>
Figure 2: Spatial coordinate workflow</p>

## What are the 3 different coordinate input forms?

### 1. Latitude, longitde decimal
<p align="center">
<kbd><img src="https://github.com/geological-survey-of-queensland/coordinate-conversion/blob/master/images/coordinate_input_form-lld.png" width="100%"></kbd><br>
Figure 3: Latitude, longitde decimal spatial coordinate input form</p>

### 2. Latitude, longitde degrees, minutes, seconds
<p align="center">
<kbd><img src="https://github.com/geological-survey-of-queensland/coordinate-conversion/blob/master/images/coordinate_input_form-dms.png" width="100%"></kbd><br>
Figure 4: Latitude, longitde degrees, minutes, seconds spatial coordinate input form</p>

### 3. Eastings, northings, zone
<p align="center">
<kbd><img src="https://github.com/geological-survey-of-queensland/coordinate-conversion/blob/master/images/coordinate_input_form-enz.png" width="100%"></kbd><br>
Figure 5: Eastings, northings, zone spatial coordinate input form</p>

## Coordinate Transformation
Text

## Coordinate Conversion
Text

## Coordinate Validation
Text

## How to store geometry in PostGIS database
1.	Geometries are stored in the PostGIS database in its native spatial format (PostGIS is part of the PostgreSQL database).
2.	Spatial data is converted on ingestion into the database (done by PostGIS).
3. The SRID = 7844 (GDA2020)

## Retention of originally entered coordinates
The database is to store the spatial coordinates and CRS as entered by the user or entered programatically. This data is for reference only and is not used for spatial display.
*	A shape file submitted to GSQ will be stored as a data object in S3.
*	A webform that captures eastings/northings/zone stores these values in the database.
*	A GeoJSON value entered in a form gets stored in the database.

### Storing coordinates in PostgreSQL database - ISO 6709 based
ISO 6709 *Standard representation of geographic point location by coordinates* is the international standard for representation of latitude, longitude and altitude for geographic point locations. The standard states:
* Fraction of degrees is preferred in digital data exchange.

## Coordinate Display - ISO 6709 based
ISO 6709 suggests the following for representation at the human interface :

1. Coordinate values (latitude, longitude, and altitude) should be delimited by spaces.
1. The decimal point is a part of the value, thus must usually be configured by the operating system.
1. Multiple points should be represented by multiple lines.
1. Latitude and longitude should be displayed by sexagesimal fractions (i.e. minutes and seconds).
1. When minutes and seconds are less than ten, leading zeroes should be shown.
1. Degree, minutes and seconds should be followed by the symbols ° (U+00B0), ′ (U+2032), and ″ (U+2033), without spaces between the number and symbol.
1. North and south latitudes should be indicated by N and S following immediately after the digits.
1. East and west longitudes should be indicated by E and W following immediately after the digits.
1. Units of elevation or depth should be given by symbols, immediately after the digits.
1. Elevation below reference level or depth above reference level should be indicated by a minus sign − (U+2212).

Examples:  
* 50°40′46.461″N 95°48′26.533″W 123,45m  
* 50°03′46.461″S 125°48′26.533″E 978.90m

## You want how many decimal places??
Sometimes we get a little carried away with the number of decimal places to which we store coordinates in a database (6 is plenty for us).

|Decimal Places|Aprox. Distance|Say What?|
|---|---|---|
|1|10 kilometers|6.2 miles|
|2|1 kilometer|0.62 miles|
|3|100 meters|About 328 feet|
|4|10 meters|About 33 feet, the the limit of accuracy for a standard GPS device|
|5|1 meter|About 3 feet|
|6|10 centimeters|Length of a cigarette, the limit of accuracy for a differential GPS device|
|7|1.0 centimeter|Width of an average fingernail|
|8|1.0 millimeter|The width of paperclip wire|
|9|0.1 millimeter|The width of a strand of hair|
|10|10 microns|A speck of pollen|
|11|1.0 micron|A piece of cigarette smoke|
|12|0.1 micron|You're doing virus-level mapping at this point|
|13|10 nanometers|Thickness of a cell wall of a bacteria|
|14|1.0 nanometer|Your fingernail grows about this far in one second|
|15|0.1 nanometer|An atom. An atom! What are you mapping?|

## See also
* [Geocentric Datum of Australia (GDA)](https://www.icsm.gov.au/australian-geospatial-reference-system)

## Licence
This code repository's content are licensed under the [Creative Commons Attribution 4.0 International (CC BY 4.0)](https://creativecommons.org/licenses/by/4.0/), the deed of which is stored in this repository here: [LICENSE](LICENSE).

## Contacts
*System owner*:  
**Mark Gordon**  
Geological Survey of Queensland  
Department of Natural Resources, Mines and Energy  
Brisbane, QLD, Australia  
<mark.gordon@dnrme.qld.gov.au>  

*Author*:  
**David Crosswell**  
Enterprise Architect  
Cross-Lateral Enterprises  
<https://crosslateral.com.au>  
