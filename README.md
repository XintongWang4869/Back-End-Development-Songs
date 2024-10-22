# Song Management Microservice: CRUD Operations with MongoDB

This project is a microservice to handle basic CRUD operations of songs from MongoDB database (a NoSQL database).

## Major Steps in Skills Network Labs

1. Set up development environment:
   
```
bash ./bin/setup.sh
exit
```

2. Start MongoDB Server. Note down the host, port, and password (needed for the following command).

3. Run the flask server:`MONGODB_SERVICE=${localhost} MONGODB_USERNAME=root MONGODB_PASSWORD=${password} flask run --reload --debugger`

4. Complete CRUD operation coding.

5. Test the RESTful APIs using curl
   
```
curl --request GET -i -w '\n' --url http://localhost:5000/song
curl --request GET -i -w '\n' --url http://localhost:5000/song/1
curl --request POST \
  -i -w '\n' \
  --url http://localhost:5000/song \
  --header 'Content-Type: application/json' \
  --data '{
        "id": 323,
        "lyrics": "Integer tincidunt ante vel ipsum. Praesent blandit lacinia erat. Vestibulum sed magna at nunc commodo placerat.\n\nPraesent blandit. Nam nulla. Integer pede justo, lacinia eget, tincidunt eget, tempus vel, pede.",
        "title": "in faucibus orci luctus et ultrices"
    }'
curl --request PUT \
  -i -w '\n' \
  --url http://localhost:5000/song/1 \
  --header 'Content-Type: application/json' \
  --data '{
        "lyrics": "yay hey yay yay",
        "title": "yay song"
    }'
curl --request DELETE \
  -i -w '\n' \
  --url http://localhost:5000/song/14 \
  --header 'Content-Type: "application/json"'
```

## Deployment

For this service to be reached from another platform like RedHat OpenShift (in addition to the Skills Network Labs):

### Install MongoDB on OpenShift

1. Open the OpenShift Web Console.      
   The console is available in the Skills Network Labs.      
   > _The Red Hat OpenShift Container Platform web console provides a graphical user interface to visualize your project data and perform administrative, management, and troubleshooting tasks. The web console runs as pods on the control plane nodes in the openshift-console project. It is managed by a console-operator pod. Both Administrator and Developer perspectives are supported._      
   > For more details, check the [official website](https://docs.openshift.com/container-platform/4.10/web_console/web-console.html) here.

2. Switch to Administrator's perspective. Click Builds, ImageStream, Create imagestream. Insert the following code:
```
#this api version is specific to openshift image management
apiVersion: image.openshift.io/v1   
kind: ImageStream
metadata:
  #the name is used to reference the ImageStream within the OpenShift cluster
  name: mongo   
spec:
  lookupPolicy:
    #the image lookup will NOT be restricted to the local OpenShift namespace.
    local: false   
  tags:
    #tracks the 'latest' version of the MongoDB image from Docker Hub
    - name: latest   
      from:
        kind: DockerImage
        name: docker.io/library/mongo:latest
```

3. Switch to Developer's perspective. Add a container image (Image stream tag from internal registry).

----

**Note**   
The mongo application is not exposed to the internet. However, you can access this server from within the pod itself:
1. `oc get po`
2. `oc exec mongo-79cbc7d7b9-5x5p9 -- mongosh --quiet --eval "db.version()"` #need to replace the example pod with your own

<br>

### Deploy to OpenShift

1. Run `oc get project`. Note down the project name. `export OPENSHIFT_PROJECT=xxx`
2. Trigger a build: `oc new-app song_repo --strategy=source --name=songs --env MONGODB_SERVICE=mongo.${OPENSHIFT_PROJECT}.svc.cluster.local --name songs`
3. Trace the logs for the previous build: `oc logs -f buildconfig/songs`
4. Expose the application to reach it from outside the lab environment: `oc expose service/songs`
5. Run `oc get route songs` to see the route, or check the web console
