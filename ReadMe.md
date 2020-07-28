# Spring Boot with camel and other useful things json-to-csv 

## To build this project use

```
mvn install
```

## To run this project with Maven use

```
mvn spring-boot:run
```

To run a test

```
curl --location --request POST 'http://localhost:8090/camel/json' --header 'Content-Type: text/plain' --data-raw '{
  "firstName": "John",
  "lastName": "Doe",
  "email": "john@mail.com",
  "age": 21
}'
```

## To deploy directly on openshift

```
mvn -P ocp fabric8:deploy
```

## For testing

```
curl http://localhost:8090/camel/api-docs
curl http://localhost:8090/camel/ping
```


## Acces Swagger UI with definition

```
http://localhost:8090/webjars/swagger-ui/index.html?url=/camel/api-docs
```

## Call the ping rest operation
```
curl http://localhost:8090/camel/restsvc/ping
```

## Run local container with specific network and IP address


```
docker stop json-to-csv
docker rm json-to-csv
docker rmi json-to-csv
docker build -t json-to-csv .
docker run -d --net primenet --ip 172.18.0.10 --name json-to-csv json-to-csv
```

Stop or launch multple instaces

```
NB_CONTAINERS=2
for (( i=0; i<$NB_CONTAINERS; i++ ))
do
   docker stop json-to-csv-$i
   docker rm json-to-csv-$i
done

docker rmi json-to-csv
docker build -t json-to-csv .

for (( i=0; i<$NB_CONTAINERS; i++ ))
do
    docker run -d --net primenet --ip 172.18.0.1$i --name json-to-csv-$i -e SPRING_PROFILES_ACTIVE=dev json-to-csv
done
```

## To release without deploying straight to an ocp cluster

```
mvn  -P ocp package
```

## To deploy using binary build on ocp

```
tar xzvf json-to-csv-ocp.tar.gz
cd json-to-csv
oc apply -f openshift.yml
oc start-build json-to-csv-s2i --from-dir=deploy --follow
```