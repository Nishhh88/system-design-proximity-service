# Option 1 :  Two-dimentional search

**Idea:**

We can get the nearby businesses by drawing a circle with the pre defined radius and find all the businesses within the circle.

Pseudo SQL Query for the same:

SELECT business_id, latitude, longitude,
FROM business
WHERE (latitude BETWEEN {:my_lat} - radius AND {:my_lat} + radius)
AND
  (longitude BETWEEN {:my_long} - radius AND {:my_long} + radius)

**Efficiency** :
The query is not efficient as the whole table will be scanned for this. Even if index is built on the longitude and latitude columns, the problem remains the presence of two dimentional data and data returned for each dimention would be huge and in order to obtain the correct data, an intersect operation needs to be performed between the two data sets. 
