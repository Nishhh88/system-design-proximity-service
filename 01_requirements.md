# Functional Overview

The system provides the functionality to search for businesses within a specified radius. 
Maximum radius allowed is 20km.
User can change the radius on UI with the options (0.5km, 1km, 2km, 5km and 20km)
Business Owners can add, delete or update a business. There is an agreement with the businesses that newly added/updated businesses will be effective the next day.
User's moving speed is slow and we don't need to constantly refresh the page.

**Functional Requirements:**

1) Return all businesses based on a user's location (longitude and latitude pair) and radius.
2) Business owners can add, delete or update a business, but this information doesn't need to be reflected in real time.
3) Customers can view detailed information about a business

**Non-Functional Requirements:**

1) Low Latency: Users should be able to see nearby businesses quickly.
2) Data Privacy: Location info is sensitive data. Therefore, we need to comply with data privacy laws like GDPR and CCPA.
3) High availability and scalability requirements. The system should be able to handle the spike in traffic during the peak hours in densely populated areas.

**Potential scalability:**
Assumption : 100 million users a day, 200 million businesses.

**QPS:** 

seconds in a day : 10^5
assume user does 5 queries per day.
Search QPS = (100 million * 5) / 10^5 = 5000
