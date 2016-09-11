# Search Rest API
The Rest Api is a public facing Rest endpoint for DMA/MA/DMC clients to search their media content.

##How does it work
It acts as a bridge between clients and elastic search server. The controller dynamically detects client and search type then sends corresponding search query to elastic search servers. It receives json responses and decorates them with Spring Hateoas. It also sends search event batches to event collector asynchronously.


##Endpoints
### DMA Search: GET /solr-dsaa/dmalive/select
This endpoint mimics solr response so that client could keep existing logic to interact as a solr client.

### MA/DMC All index search: GET /search-engine/v1/search/{client}/all
This endpoint returns search result from all indexes (movie, cast, genre, video).

### MA/DMC movie search: GET /search-engine/v1/search/{client}/movie
This endpoint returns search result from movie index.

### MA/DMC genre search: GET /search-engine/v1/search/{client}/genre
This endpoint returns search result from genre index.

### MA/DMC cast search: GET /search-engine/v1/search/{client}/cast
This endpoint returns search result from cast index.

### MA/DMC video clip search: GET /search-engine/v1/search/{client}/video
This endpoint returns search result from video index.


##Deployment
Search api is dockerized and terraformed.

Use the following command to run the docker image. SPRING_PROFILES_ACTIV can be used to define the environment. Corresponding configuration is retrieved from spring config server based this variable. 
```{r, engine='bash', count_lines}
docker run -ti --net=host -d -p 8080:8080 -e profile=${var.search_profile} -e SERVER_PORT=8080 -e SPRING_CLOUD_CONFIG_URI=http://${var.search_profile}.recommender.config.server.wdsds.net:8888 chenp077/de-search-api-${var.search_profile}
```
More details in deployment repo.