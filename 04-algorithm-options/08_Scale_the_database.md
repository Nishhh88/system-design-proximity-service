# Scale the database

The scope here is to scale the business table and the geospacial index table.

**Business Table**
The data for the business table may not all fit in one server, so sharding will help. The easiest approach would be to shard everything by the business ID. This sharding scheme will ensure that load is evenly distributed among all the shards, and it is easy to maintain operationally.

**Geospacial Index Table**
For Geohash implementation:
* Option 1: For each geohash key, there is a JSON array of business IDs in a single row, This means all businesses with the same geohash are saved in the same row.
* Option 2: If there are multiple businesses in the same geohash, there will be multiple rows, one for each business.

The better option is Option 2 because for the option 1, to update (add, remove, edit) a business, the whole array of businesses belonging to one geohash will need to be updated. We also need to lock to handle concurrent updates. While, on the option 2, the removal and addition of a business is very simple. There wouldn't be need to lock anything.

**Scale the geospacial index**
We need to consider if our data is large enough to be sharded. In our case, quadree only takes (1.7 GB), this can be saved in any database server. Also, sharding will require adding logic inside the application. But, only one server might not be able to handle the read load, in this case, it is necessary to distribute the read load among multiple database servers. Therefore, the recommended approach would be to scale the geospacial index through replicas.

