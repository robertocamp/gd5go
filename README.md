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
## local development
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
## push image to ECR
1. login:  `aws ecr get-login-password --region us-east-2 | docker login --username AWS --password-stdin 240195868935.dkr.ecr.us-east-2.amazonaws.com`
2. tag: `docker tag 021c0d4a2224 240195868935.dkr.ecr.us-east-2.amazonaws.com/docker-gs-ping:0.0.1`
3. list:  `docker image ls`
4. push: `docker push 240195868935.dkr.ecr.us-east-2.amazonaws.com/docker-gs-ping:0.0.1`

## links
- https://docs.aws.amazon.com/AmazonECR/latest/userguide/repository-create.html
- https://docs.aws.amazon.com/AmazonECR/latest/userguide/docker-push-ecr-image.html
