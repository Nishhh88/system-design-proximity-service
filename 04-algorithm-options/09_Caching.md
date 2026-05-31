# Caching

The first question that needs to be asked is if cache is even required. 

* In this case, the workload is read heavy and is relatively small which could fit in any modern database server. Threfore, the queries are not I/O bound and should run almost as fast as in-memory cache.
* If the read performance is a bottleneck, we can add database read replicas to improve the read throughput.

Although, caching is not an ideal approach here as it will add to the bench marking and cost analysis. If still caching was to be implemented the below approach should be followed.

**Cache Key**
If the location key coordinates are chosen as the cache key, it would have below issues:

* Location coordinates returned from a mobile phone are not accurate, even without moving, different coordinates will be returned for the same location.
* A user can move from one location to another causing location coordinates to change slightly. While, for the application, this change wont be meaningful.

**Type of data to Cache**
There can be two types of data that can be chached: geohash, business_id

* Caching the list of business Ids in a grid:
  Since, the business data is relatively stable, the data for a geohash can be precomputed and stored in a key-value store such as Redis.

  Below is an example for getting the nearby businesses with caching enabled:
  1) Get the list of business Ids for a given geohash.
     SELECT business_id FROM geohash_index WHERE geohash LIKE '{:geohash}%
  2) Store the result in the Redis Cache if cache misses.
     public List<String> getNearbyBusinessIds(String geohash) {
       String cacheKey = hash(geohash);
       List<String> listOfBusinessIds = Redis.get(cacheKey);
       if(listOfBusinessIds == null) {
         listOfBusinessIds = Run the select SQL query above;
         Cache.set(cacheKey, listOfBusinessIds, "1d"); 
     }
      return listOfBusinessIds;   
  }

  
