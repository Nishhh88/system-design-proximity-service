# Option 5: Google S2

This is also an in-memory solution. It uses Hilbert curve for implementation. S2 library can be used for the implementation.

**Hilbert Curve**
A hilbert curve converts the 2D data into 1D data, while keeping the nearby points mostly together. Databases are good at giving the indexing solutions for 1D data but are not so good for 2D data. A hilbert curve draws a continuous line that visits every cell atleast once. 

* S2 library is widely used across various companies like Google, Tinder etc.
* This library is widely used geofencing because it can cover arbitrary areas with varying levels. (Like, in our use case, geofence is a virtual perimeter for a real world geographic area. A geofence could be dynamically generated, by changing the radius around a point location, or can be a predefined set of boundaries like a school). Geofencing allows us to define perimeters that surround the areas of interest and to send notifications to users who are out of those areas. Therefore, delivering with richer functionalities than just returning the nearby businesses.

* Another advantage is its Region Cover algorithm. Instead of having a fixed level like geohash, min level, max level and max cells can be specified. Therefore, the results returned are more granular.

