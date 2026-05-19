# Oprion 3: Geohash

Geohasg would be a better alternative to the evenly divided grid option. As, it works by reducing the two dimentional longitude and latitude data into a one-dimentional string of letters and digits. Geohasg algorithms work by recursively dividing the world into smalled and smaller grids with each additional bit. 

**Working of Geohash**
First the planet would be divided into four quadrants along with the prime meridian and equator:

                                              01    |    11
                                              ---------------
                                              00    |    10

Now to further enhance the granularity, each grid is divided into four smaller grids. Each grid is being represented by alternating between longitude bit and latitude bit.

                                            01 01 | 01 11 | 11 01 | 11 11
                                            ------------------------------
                                            01 00 | 01 10 | 11 00 | 11 10
                                            ------------------------------
                                            00 01 | 00 11 | 10 01 | 10 11
                                            ------------------------------
                                            00 00 | 00 10 | 10 00 | 10 10 

This can be repeated until the desired is obtained for the grid size. Geohash usually uses base32 representation. 

Geohasg has 12 levels. The precision factor determines the size of the grid. For our case, the size of the planet can be divided into the 12 precisions.

geohash length  |  Grid width * height
        1       |  5009.4 km * 4992.6km
        2       |  1252.3 km * 624.1 km
        3       |  156.5 km * 156 km
        4       |  39.1 km * 19.5 km
        5       |  4.9 km * 4.9 km
        6       | 1.2 km * 609.4 km
        7       | 152.9m  * 152.4m
        8       | 38.2m * 19m
        9       | 4.8m * 4.8m
        10      | 1.2m * 59.5cm
        11      | 14.9 cm * 14.9 cm
        12      | 3.7 cm * 1.9 cm

For our user story, the level 4-6 would be accurate. But, this has some edge scenarios that need to be considered.

**Boundary Issues**

**Boundary Issue 1**
Geohashing guarantees that the longer a shared prefix is between two geohashes, the closer they are. However, the reverse is not true: two locations can be very close but not have any shared prefix at all. This will happen when two locations belong to different halves of the world but are still close as Earth is round. Because of this issue, a simple SQL query below would fail to fetch all nearby businesses.
SELECT * FROM geohash_index WHERE geohash LIKE '9q8zn%';


                                        
