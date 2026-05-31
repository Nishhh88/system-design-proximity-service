# Geohash vs Quadtree

**Geohash**
* Easy to build
* Supports running businesses within a specified radius
* When the precision(level) of geohash is fixed, the size of the grid is fixed as well. The implementation in more populous areas becomes complex.
* Updating of the index in the database is easy, for example, to remove a business, just remove the corresponding row from the database.

**Quadtree**

* Harder to implement comparatively due to the tree structure.
* Supports returning k-nearest businesses, eg if you are running out og gas, you would care about finding the k-nearest businesses instaed of businesses within a radius.
* Dynamically adjusts the grid size based on the population in a region.
* Updating the index is more complex, as it requires update in db as well as inserting/removing the business from the db which can also result in updation of the tree structure itself based on the number of businesses. Also, needs locking mechanism to be implemented if multiple threads are being used. (A possible fix for rebalancing if a leaf node has no more space would be to over-allocate the ranges).
  
