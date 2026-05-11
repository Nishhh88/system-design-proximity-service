RESTful APIs are being used 

GET /v1/search/nearby

Request Parameters:

Field     | Description                                      | Type 
latitude  | Latitude of a given location                     | decimal
longitude | Longitude of a given location                    | decimal
radius    | Optional. Default is 5000 etres (about 3 miles)  | int

Response:

{
  "total" : 10,
  "businesses" : [{business object}]
}

The business object contains everything required to render the search result page. But, still additional atributes such as pictures, reviews, star ratings etc. will be required. Therefore, whenever a user clicks on a business, a new endpoint call to fetch the detailed information of a business is required.

APIs for Business:

API                           | Detail                                          |
GET /v1/businesses/:id        | Return a detailed information about a business  |
POST /v1/businesses           | Add a business                                  |
PUT /v1/businesses/:id        | Update details of a business                    |
DELETE /v1/businesses/:id     | Delete a business                               |

Data Model (read/write ratio, schema design, scalability of the database):

Read/Write ratio:
Read volume is high because the following two features are very commonly used:
Search for nearby businesses
View the detailed information of a business

Write volume is low because adding, removing, and editing business info are infrequent operations.

For a read heavy operation, MySQL would be a good fit.

Data Schema:
Two tables will be used, business table and geospacial index table.

Business Table:
This table contains detailed information about a business. 

business
business_id     PK
address
city
state
country
latitude
longitude

Geo Index table:
This table is used for efficient proccessing of spacial operations. 



