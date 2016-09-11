# Redirect Rest API
The redirect endpoint is a internal api that handles click event and redirection

##How does it work
The endpoint is embedded in each search result. Everytime clients click on any of search results, it triggers a callback to redirection service. It sends a click event which contains orginal serach term, all search results, as well as the result client selects to event collector. It is designed to collect user actions to support other feature like predictive search. It also handles redirection if required.


##Endpoints
### Redirect: GET/POST /redirect
* Required param:
d: base 64 encoded json string (click event)
* Optional param:
redirect: boolean, handles redirection if true

##Deployment
Redirection api is dockerized and terraformed.

Use the following command to run the docker image. SPRING_PROFILES_ACTIV can be used to define the environment. Corresponding configuration is retrieved from spring config server based this variable. 
```{r, engine='bash', count_lines}
docker run -ti --net=host -d -p 8080:8080 -e profile=${var.search_profile} -e SERVER_PORT=8080 -e SPRING_CLOUD_CONFIG_URI=http://${var.search_profile}.recommender.config.server.wdsds.net:8888 chenp077/de-search-redirect-${var.search_profile}
```
More details in deployment repo.