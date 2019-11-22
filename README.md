# Kubernetes-Dockerize-GolangChatApp
Clone the application, make it dockerized and orchestrate it through kubernetes

Clone the simple golang+vue.js chat web application.

```
git clone git@github.com:ayesha54/Chat-application.git
cd Chat-application
touch Dockerfile
subl Dockerfile

```

I am using sublime text editor, you can use any other in which you are comfortable with.

Dockerfile

```
# Dockerfile References: https://docs.docker.com/engine/reference/builder/

# Start from the latest golang base image
FROM golang:latest

# Add Maintainer Info
LABEL maintainer="Ayesha Kaleem"

# Set the Current Working Directory inside the container
WORKDIR /app

# Copy go mod and sum files
COPY go.mod go.sum ./

# Download all dependencies. Dependencies will be cached if the go.mod and go.sum files are not changed
RUN go mod download

# Copy the source from the current directory to the Working Directory inside the container
COPY . .

# Build the Go app
RUN go build -o main .

# This container exposes port 8080 to the outside world
EXPOSE 8000

# Run the binary program produced by `go install`
CMD ["./main"]

```

Now, build the image of a docker file, login at docker.io and push it on to docker registry which is docker hub.

``` 
docker build . -t your-name/chat-application 
docker images
docker login docker.io

```
Make repo on docker hub and  and rename the image according to the docker hub

``` 
docker tag your-name/chat-application your-dockerhubUserName/repoName 
docker push your-dockerhubUserName/repoName

```
Your image is ready at docker hub <3
your-dockerhubUserName/repoName = image_name

Now, lets play with kubernetes. For this, we use minikube locally

```
minikube start
kubectl run chat-app --image=image_name --port=8000
kubectl get deploy
kubectl get po

```
Now expose application service,

```
kubectl expose deploy chat-app --type=NodePort --port=8000
kubectl get services
minikube service chat-app --url

```
open the url in browser for example , http://192.168.64.3:30936

Now, Scale the application 

```
kubectl scale --replicas=4 deployment/chat-app
kubectl get all
```
