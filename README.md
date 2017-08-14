
1.	Install docker compose : 

https://docs.docker.com/compose/install/#install-as-a-container


2.	Clone from xxxxxxxxx


3.	Bring up the containers 

docker-compose up

4.	If you would like to bring down the containers, it’s docker-compose down

5.	For changing the squid.conf, go into the squid directory. You’ll have to rebuild the container for the changes to be picked up.  

docker-compose build

6.	For finding the IP of the container, issue a `docker ps` and then use the container name like so : 


docker inspect --format='{{range .NetworkSettings.Networks}}{{.IPAddress}}{{end}}' $INSTANCE_ID

docker inspect --format='{{range .NetworkSettings.Networks}}{{.IPAddress}}{{end}}' elasticsearch

Alternatively, you could do a `docker inspect $INSTANCE_ID` and eye-ball to find the IP. 

(More help at https://docs.docker.com/engine/reference/commandline/inspect/#examples)

7.	To bash access into the container,  

docker exec -it $INSTANCE_ID bash


In order to verify whether all is good, 

Use the Unauthenticated proxy to hit ElasticSearch

curl http://172.18.0.2:9200 --proxy http://localhost:3228

```
{
  "name" : "ncf-AeL",
  "cluster_name" : "docker-cluster",
  "cluster_uuid" : "XejawPrbQq6f5aFxMAMGMg",
  "version" : {
    "number" : "5.5.1",
    "build_hash" : "19c13d0",
    "build_date" : "2017-07-18T20:44:24.823Z",
    "build_snapshot" : false,
    "lucene_version" : "6.6.0"
  },
  "tagline" : "You Know, for Search"
}
```

curl http://172.18.0.2:9200 --proxy http://localhost:3328 --proxy-user squid:changeme

```
{
  "name" : "ncf-AeL",
  "cluster_name" : "docker-cluster",
  "cluster_uuid" : "XejawPrbQq6f5aFxMAMGMg",
  "version" : {
    "number" : "5.5.1",
    "build_hash" : "19c13d0",
    "build_date" : "2017-07-18T20:44:24.823Z",
    "build_snapshot" : false,
    "lucene_version" : "6.6.0"
  },
  "tagline" : "You Know, for Search"
}
```


curl http://172.18.0.2:9200 --proxy http://localhost:3328 --proxy-user squid:XXXYYYYKKKK

##### HTML OF ACCESS DENIED PAGE



(You wouldn’t be able to access the 172.* IP from your terminal)

