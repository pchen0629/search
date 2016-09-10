# Event Collector REST API
The Rest Api is a public facing Rest endpoint for custom clients and devices to submit events that satisifies predfined avro schemas. 

##How does it work
As Json events being submited to a predefined endpoint, json blob are validated against a predefined schema associated with the endpoint. If the event is valid, it will be send to a kafka topic associatd with the endpoint in avro format. 
The predefined schema is stored in a schema registry which can be retrieved by downstream processes when processing the data.

New endpoint can be created by submit new schemas to the event collector.  If the end point already exists,  and the new schema is compatiable to the old one, a new version of the schema will be created for the same endpoint. If the two schema are not compatiable, an error will be returned. User will need to submit that schema under a different name. 

Endpoing path are mapped directly to the name of schema and kafka topics. 
For example: http://<event collector domain>/event/playback/start maps directly to playback.start schema name in the schema registry and kafka topic name.

Raw events are encrypted with AES when transmitted over the wire on public net. So a shared secret will be distributed to all clients, when submit the events, the client must 

1. apply sha-1 on the share secret. Take the first 16 bytes of the sha as the AES encryption key. 
2. encrypt the json event with AES/ECB/PKCS5Padding algorthm and first 16 bytes of AES encryption key. 
3. apply BASE64 ecoding 
4. submitting to the /event/{eventCategory}/{eventType} endpoint. 

Event is submited in asynchronized fashion by default, client can force synchronized submission by setting async=false in the event submission api. Partition and index information will not be returned for async submission.



##Alternative Projects
Confluent IO comes with its own rest poxy. However, it is not as efficient as the API provided by the event collector. 
Confluent IO checks for schema existence and compatibility on every event submission, it works better with large batch of events. 
The event collector has lower latency when processing both batch and single events. 


##Prepare the Development Environment

run the following command to start zookeeper, kafka, schema registry in local dev environment.  It automatically downloads confluent.io distribution
```{r, engine='bash', count_lines}
./start-local-schema-registry.sh clean
```

run the following command to stop all
```{r, engine='bash', count_lines}
./stop-local-schema-registry.sh
```

##Endpoints
###Create a new Schema PUT /schema/<event.type>
Create a new endpoint to consume events that can be validated against the schema uploaded. Dot can be used in the event type to * define the name space of the event type. 
* required parameters: partitions, replica

###Post events POST /events/<event.type>
Up on submitting a schema, an end point of the same name is created. 
* required headers: retyCount, sentTime, User-Agent, Content-Type
* optional headers: senderTags
* When content-type is application/json-encrypted, the endpoint expect the event being encrypted with and AES agorithm. otherwise raw json file can be send.


##Deployment
Event collector creates a docker image as part of the maven package process. 
Use the following command to run the image. SPRING_PROFILES_ACTIV can be used to define the environment. Corresponding configuration is retrieved from spring config server based this variable. 
```{r, engine='bash', count_lines}
docker run -p 8000:8080  -e "SPRING_PROFILES_ACTIVE=production" -e "SPRING_CLOUD_CONFIG_URI=http://dev.config-server.wdsds.net:8888"  springio/event-collector-api
```

docker-machine create --driver amazonec2  --amazonec2-region us-west-2 --amazonec2-instance-type m3.large --amazonec2-security-group dev-event-collector --amazonec2-ssh-keypath ~/.ssh/data-service-dev.pem --amazonec2-secret-key **** --amazonec2-vpc-id vpc-3011f054 -amazonec2-subnet-id  subnet-39313e4e  aws-eventcoll02