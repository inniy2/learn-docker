## Docker building images  
- - - -  
```bash
cd ~/git/go-demo

cat Dockerfile

# Docker image build will fail because there is no go-demo binary
docker image build -t go-demo .

```

> cat Dockerfile:  
```bash
# Alpine is the smallest Linux destro
FROM alpine:3.4
# Maintainer or creator
MAINTAINER 	Viktor Farcic <viktor@farcic.com>

# Execute shell inside the container
RUN mkdir /lib64 && ln -s /lib/libc.musl-x86_64.so.1 /lib64/ld-linux-x86-64.so.2

# Running port for container
EXPOSE 8080

# Environment value. You can replace the value at runtime
ENV DB db
ENV SERVICE_NAME go-demo

# Command ?
CMD ["go-demo"]

# Docker health checkup
HEALTHCHECK --interval=10s CMD wget -qO- localhost:8080/demo/hello

# Copy local binary to the container
COPY go-demo /usr/local/bin/go-demo

# Execute shell inside the container
RUN chmod +x /usr/local/bin/go-demo
```
##### How to create go-demo binary  
```bash
# -w write output to the dir
docker container run -it --rm -v $PWD:/tmp -w /tmp golang:1.6 \
sh -c "go get -d -v -t && go build -v -o go-demo"

ls -al


# Chown from root to me
sudo chown XXX:XXX go-demo

# Try again to create docker image
docker image build -t go-demo .

docker image ls
```


> Why do you need another container for build a binary files?  
> solution is :  
> cat Dockerfile.big  
```bash
# Use golang 1.7 as a base image to build a image
FROM golang:1.7 AS build

# Add current directory files to /src
ADD . /src

# cd /src
WORKDIR /src

# build go-demo with golang 1.7
RUN go get -d -v -t
RUN go test --cover -v ./...
RUN go build -v -o go-demo

EXPOSE 8080
ENV DB db
CMD ["go-demo"]
HEALTHCHECK --interval=10s CMD wget -qO- localhost:8080/demo/hello

# move /src/go-demo to /usr/local/bin/go-demo
RUN mv go-demo /usr/local/bin/go-demo

RUN chmod +x /usr/local/bin/go-demo
```

##### Build a image with a custome build file  
```bash
docker image build -f Dockerfile.big -t go-demo .

docker image ls
```
> Is every thing good? No  
> Unlike Alpine golang 1.7 is almost 800MB (Alpin build was 28MB)  
> It is always better to have slimp image for a production usage  
> solution is:  
> cat Dockerfile.multistage  
```bash
FROM golang:1.7 AS build
ADD . /src
WORKDIR /src
RUN go get -d -v -t
RUN go test --cover -v ./...
RUN go build -v -o go-demo



FROM alpine:3.4
MAINTAINER 	Viktor Farcic <viktor@farcic.com>

RUN mkdir /lib64 && ln -s /lib/libc.musl-x86_64.so.1 /lib64/ld-linux-x86-64.so.2

EXPOSE 8080
ENV DB db
CMD ["go-demo"]
HEALTHCHECK --interval=10s CMD wget -qO- localhost:8080/demo/hello

# here is the trick!! get a binary from a multistaging container
COPY --from=build /src/go-demo /usr/local/bin/go-demo
RUN chmod +x /usr/local/bin/go-demo
```


##### Build a image with a mutlstage container  
```bash
docker image build -f Dockerfile.multistage -t go-demo .

docker image ls
```
> Now, we have a slimp image at once  
> for more information regarding building a image  
[Docker file reference](https://docs.docker.com/engine/reference/builder/)  
