# gd5go
> basic golang web application using Fiber framework
## Docker images
- Docker images can be inherited from other images. 
- Therefore, instead of creating our own base image, we’ll use the official Go image that already has all the tools and packages to compile and run a Go application. 
- You can think of this in the same way you would think about class inheritance in object oriented programming or functional composition in functional programming.
- The docker build command creates Docker images from the Dockerfile and a “context”. 
- A build context is the set of files located in the specified path or URL. 
- The Docker build process can access any of the files located in the context.
- The build command optionally takes a --tag flag. 
- This flag is used to label the image with a string value, which is easy for humans to read and recognise. 
- If you do not pass a --tag, Docker will use latest as the default value.
### image tagging in Docker
- An image name is made up of slash-separated name components. 
- Name components may contain lowercase letters, digits and separators. 
- A separator is defined as a period, one or two underscores, or one or more dashes. 
- A name component may not start or end with a separator.
- An image is made up of a manifest and a list of layers. 
- In simple terms, a “tag” points to a combination of these artifacts, aka a "version" or "recipe" 
- You can have multiple tags for the image and, in fact, most images have multiple tags. 
- `docker image tag docker-gs-ping:latest docker-gs-ping:v1.0`
   + The Docker tag command creates a new tag for the image. 
   + It does not create a new image. 
   + The tag points to the same image and is just another way to reference the image.
## project intialization
1. create project in Github
2. `git clone git@github.com:robertocamp/gd5go.git`
3. `go mod init github.com/roberto_camp/gd5go`
  - Usually the very first thing you do once you’ve downloaded a project written in Go is to install the modules necessary to compile it.
4. `touch main.go`
5. `touch Dockerfile`
## Docker image example: build and push
1. start the Mac docker daemon
2. copy src code from https://docs.docker.com/language/golang/build-images/ into main.go
3. `go get github.com/labstack/echo/v4`
4. `go get github.com/labstack/echo/v4/middleware`
5. go run main.go
6. connect to localhost:8080 in browser
7. develop Dockerfile
8. **local smoke test**
  - `docker build --tag docker-gs-ping .`
  - `docker image ls`
9. **multistage**
  - `docker build -t docker-gs-ping:multistage -f Dockerfile.multistage .`
### push image to ECR
1. login:  `aws ecr get-login-password --region us-east-2 | docker login --username AWS --password-stdin 240195868935.dkr.ecr.us-east-2.amazonaws.com`
2. tag: `docker tag 021c0d4a2224 240195868935.dkr.ecr.us-east-2.amazonaws.com/docker-gs-ping:0.0.1`
3. list:  `docker image ls`
4. push: `docker push 240195868935.dkr.ecr.us-east-2.amazonaws.com/docker-gs-ping:0.0.1`

## gdgo5 development
1. delete previous go.mod, go.sum files from demo/example 
2. re-run `go mod init github.com/roberto_camp/gd5go`
3. create main.go with basic fiber server
4. `go run main.go`  --make sure site comes up on localhost:3000
5. create Dockerfile
6. start local Docker daemon on mac (app icon)
7. `docker build --tag gd5go .`
8. `docker image ls`  --verify image size
### local smoke test
- Dockerfile should be able to build image for running on M1 Mac
- `docker run -p 3000:3000 gd5go`  (3000 is the container port ; 3000 is the browswer port)
- test in browser: localhost:3000 
### push image to ECR
9. create ECR repo for gd5go: `240195868935.dkr.ecr.us-east-2.amazonaws.com/gd5go`
10. tag image for push: `docker tag f48fdc146ae7 240195868935.dkr.ecr.us-east-2.amazonaws.com/gd5go:0.0.1`
11. push image to ECR: `docker push 240195868935.dkr.ecr.us-east-2.amazonaws.com/gd5go:0.0.1`
12. create namespace: `k create -f namespace.yml` (see namespace file in ./k8s)
13. create deployment, service, ingress: k apply -f gd5go-eks.yml
## mongodb
### install dependencies
go get go.mongodb.org/mongo-driver/bson
go get go.mongodb.org/mongo-driver/bson/primitive
go get go.mongodb.org/mongo-driver/mongo
go get go.mongodb.org/mongo-driver/mongo/options
### install mongodb (Homebrew)
`brew tap mongodb/brew`
`brew install mongodb-community@6.0`
`mongod --config /opt/homebrew/etc/mongod.conf --fork`
`ps aux | grep -v grep | grep mongod`
`mongosh`
`db.createCollection("fiber_test");`
`use fiber_test`
`show dbs`
### default database
- The default database in MongoDB is test. If you do not create a new database, the collection is stored in a test database.
- **Note:** In MongoDB, collections are created only after inserting content! That is, after creating a collection (data table), a document (record) is inserted and actually a collection is created.

use fiber_test

db.createUser(
  {
    user: "golangdev",
    pwd: "password",
    roles: [ { role: "readWrite", db: "fiber_test" }]
  }
);

## links
- https://docs.aws.amazon.com/AmazonECR/latest/userguide/repository-create.html
- https://docs.aws.amazon.com/AmazonECR/latest/userguide/docker-push-ecr-image.html
- https://kb.objectrocket.com/mongo-db/mongodb-create-database-username-password-to-secure-data-467
