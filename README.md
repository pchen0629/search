# de-search

The search project aims to provide users the quickest way of finding the movies/video clips they want, similar to what users experience in Netflix and other video platforms. It is designed to provide a stable, fast, and accurate tool to help users get more relevant information from the large volume of videos and content that is available.

##Features
1. Provides lightning fast, scalable, and resilient search api
2. Supports multitenancy
2. Provides real-time document indexing
3. Provides customizable UI that justifies search result
4. Automated deloymentment 
5. Monitoring dashboard
6. Supports async batch submission to event collector
7. centralized configuration 

##Submodules and Dependencies
More design, build and deploy details can be found in each of the submodules below. 

1. Client facing search [rest API](web-api/)
2. Customizable [UI](search-kit-ui/)
3. Elastic Search [indexing job](elastic-search-indexing/)
4. [Event publishing](common/) service
5. automated [deployment](https://github.disney.com/chenp077/de-search-deployment)
6. API monitoring [dashboard](https://github.com/Netflix/Hystrix/tree/master/hystrix-dashboard) by netflix
7. Centrailzed [configuration server](https://github.com/spring-cloud/spring-cloud-config) by spring cloud
8. Eureka [service registry](https://github.com/Netflix/eureka) by netflix 


##Owner
Disney Studios Data & Platform Engineering Team

##Contributer
Pei Chen <pei.chen@disney.com>

##Contributing
1. Fork it!
2. Create your feature branch: `git checkout -b my-new-feature`
3. Commit your changes: `git commit -am 'Add some feature'`
4. Push to the branch: `git push origin my-new-feature`
5. Submit a pull request :D