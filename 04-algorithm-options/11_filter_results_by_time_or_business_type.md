# Filter results by time or business type

Follow up question : How to return businesses that are open now, or only return businesses that are restaurants.

When the world is divided into small grids with geohash or a quadtree, the number of businesses returned from the search result is relatively small. Therefore, it is acceptable to return business IDs first, hydrate business objects, and filter them based on the opening time or business type. The solution assumes the opening time and business type are stored inside the business table.  
