# Search Rest API
The Rest Api is a public facing Rest endpoint for DMA/MA/DMC clients to search their media content.

##How does it work
It acts as a bridge between clients and elastic search server. The controller dynamically detects client and search type then sends corresponding search query to elastic search servers. It receives json responses and decorates them with Spring Hateoas. It also sends search event batches to event collector asynchronously.


##Endpoints
### DMA Search GET /solr-dsaa/dmalive/select
This method mimics solr response so that client could keep existing logic to interact as a solr client
* Required param:
** q: search text
** fl: fields of result, must contain dsaa_guid
* Optional param:
rows: max number of results

### MA/DMC All index search GET /search-engine/v1/search/{client}/all
This method returns search result from all indexes (movie, cast, genre, video)
* Required param:
query: search string
* Optional param:


##Deployment
Search api is dockerized.

Use the following command to run the dojcker image. SPRING_PROFILES_ACTIV can be used to define the environment. Corresponding configuration is retrieved from spring config server based this variable. 
```{r, engine='bash', count_lines}
docker run -ti --net=host -d -p 8080:8080 -e profile=${var.search_profile} -e SERVER_PORT=8080 -e SPRING_CLOUD_CONFIG_URI=http://${var.search_profile}.recommender.config.server.wdsds.net:8888 chenp077/de-search-api-${var.search_profile}
```