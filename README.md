## Deploy wiki-demo (Vert.x) microservice on Openshift
### Deploy project via oc CLI

#### Basic project creation
-----------
To create app:
```
$ git clone https://github.com/andreessen/vertx-wiki-example
$ cd vertx-wiki/
$ oc new-build --binary --name=vertx-wiki -l app=vertx-wiki
$ mvn package; oc start-build vertx-wiki --from-dir=. --follow
$ oc new-app vertx-wiki -l app=vertx-wiki,hystrix.enabled=true
$ oc expose service vertx-wiki
```

#### Enable Jolokia and Readiness probe
-----------
```
$ oc set env dc/vertx-wiki AB_ENABLED=jolokia; oc patch dc/vertx-wiki -p '{"spec":{"template":{"spec":{"containers":[{"name":"vertx-wiki","ports":[{"containerPort": 8778,"name":"jolokia"}]}]}}}}'
$ oc set probe dc/vertx-wiki --readiness --get-url=http://:8080/api/health
```

