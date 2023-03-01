---
layout: post
title:  Docker Compose, bundle up service and magic things will happen automatically"
date:   2023-03-01
desc: "I am happy to learn about docker compose and link the services with its powerful options all the magic things happens with simple commands. Lets start our learning with it."
---

#### Docker Compose:
 If we are running application with multiple service, better way to use docker compose. docker compose is accompolished with configuration with YAML files
 docker-compose.yml files holds different services and its options to run the services.
 once docker-compose.yml is ready *docker-compose up* simple command will bring all the mentioned service up.


###Link the Container:
 Each services are independent and it is necessary to link the services to have better communciation.
*docker run -d –name =vote -p 5000:80 –link redis:redis voting-app*
 It creates entry in voting app container /etc/hosts redis and internal ip of redis ccontainer.

--link will deprecating with advanced configuration. docker-compose gives good control over links and network config.
### docker-compose version: 1
{%highlight docker%}

 redis:
   image: redis
db:
    image: postgres:9.4
vote:
    image ./vote
    port:
-	5000:80
    links:
-	redis
result:
   image: result
  ports:
-	5001:80
  Links:
-	db
 worker:
       image: worker
       links:
         -	db
         -	redis

{%endhighlight%}

####Docker compose version 2 – links will be automaticalluy made.
{%highlight docker%}
 version: 2
 services:
       redis:
          image: redis
         networks:
            -back-end 
        db:
            image: postgres:9.4
           networks:
                        -back-end
        vote:
            image ./vote
            port:
        -	5000:80
        networks:
               -front-end
              -back-end:
        result:
           image: result
          ports:
        -	5001:80
    	networks:
          -front-end
        	-back-end
    worker:
           image: worker
      networks:
           front-end:
          back-end:

{%endhighlight%}
