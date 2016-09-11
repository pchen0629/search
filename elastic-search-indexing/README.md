# Elastic Search Indexing
Elastic Search Indexing is a spring batch job that gets movie metadata updates from VMS (To be replaced by data from blueprint) and apply the updates to elastic search indexes.

##How does it work
There are two parts of this job:
* Reader: get updated movies from DMA catelog api for certain categories
* Writer: transform raw data to desired format and update indexes in near real time fashion via elastic search api

##Deployment
It is a daily job deployed on azkaban job scheduler.