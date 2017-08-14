# Installation

### 1. Install docker compose

https://docs.docker.com/compose/install/#install-as-a-container

### 2. Clone from Repo

```
git clone https://github.com/arunma/elastic-behind-a-proxy.git
```

### 3.	Bring up the containers 

```
docker-compose up
```

### 4. If you would like to bring down the containers

```
docker-compose down

```

### 5. Finding IP of the container 
_(coz I am unable to make the DNS work)_

Issue a `docker ps` and then use the `inspect` against the container name/container id : 

`docker inspect --format='{{range .NetworkSettings.Networks}}{{.IPAddress}}{{end}}' $INSTANCE_ID`

eg. `docker inspect --format='{{range .NetworkSettings.Networks}}{{.IPAddress}}{{end}}' elasticsearch`

Alternatively, you could do a `docker inspect $INSTANCE_ID` and eye-ball to find the IP. 

(More help at https://docs.docker.com/engine/reference/commandline/inspect/#examples)

# Verification

### Using the unauthenticated proxy to hit ElasticSearch

```
curl http://172.18.0.2:9200 --proxy http://localhost:3228
```
##### Response :

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

Note that I've hardcoded the password as `changeme` in the ./squid_auth/passwd file.

```
curl http://172.18.0.2:9200 --proxy http://localhost:3328 --proxy-user squid:changeme
```

##### Response : 

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

### Wrong password check

```
curl http://172.18.0.2:9200 --proxy http://localhost:3328 --proxy-user squid:XXXYYYYKKKK
```
##### Response

_HTML OF ACCESS DENIED PAGE_


(You wouldn’t be able to access the 172.* IP from your terminal)

