The system involves two services, location based service and business related service



                                                  user
                                                    |
                                                    |
                                                    |
                                                Load Balancer
                                                  /        \
                                                /           \
                                          /search/nearby   /businesses/{:id}
                                             /                 \
                                           LBS               Business Service
                      /                 /    |                     |
                     Read             Read    Read                  write
                      /              /       |                      \
                 Replica        Replica       Replica----Replicate---Primary DB instance
                     \              \                                     /     /         
                      \           Replicate------------------------------      /
                   Replicate-------------------------------------------------/ 


Load Balancer:
The load balancer automatically distributes incoming traffic across multiple services. We have a single DNS entry point and it internally routes the API calls to the appropriate services based on the url paths.

Location Based Service:
The service is responsible for finding the nearby businesses for a given radius and location. The LBS has following characteristics:
  - It is a read-heavy request with no write requests.
  - QPS is high, especially during peak hours in dense areas.
  - This service is stateless so it's easy to scale horizontally.

Business Service:
This service deals with two types of requests:
  - Business owners create, update or delete businesses. The requests are mainly write operations, and the QPS is not high.
  - Customers view detailed information about a business. QPS is high during peak hours.

Database Cluster:
The database will have a primary-secondary setup. In this setup, the primary database handles the write operations, the multiple replicas are used for the read operations. Data is saved to the primary database first and then replicated to replicas. Due to the replication delay, there might be some discrepency between data read by the LBS and the data written to the primary database. This is not an issue because the business information doesn't need to be updated in real time.

Scalability of business service and LBS:
Both the business service and LBS are stateless services, so its easy to automatically add more to servers to accomodate peak traffic (eg. mealtime) and remove servers during off-peak hours (eg. sleep time). If the system operates on the cloud, different regions and availability zones can be added to further improve availability. 
