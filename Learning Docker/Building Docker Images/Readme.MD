- *docker build -t name-of-result .* (here . is a current location) 

##### 1 Building Dockerfiles
- example 1 : hello container
  Dockerfile content : \
    *FROM busybox*\
    *RUN echo "building simple docker image"*\
    *CMD echo "hello container"* 
  
    On Terminal :\
    *docker build -t hello .*\
    *docker run --rm hello*

- example 2 : installing program
  
  Dockerfile content : \
    *FROM debian:sid*\
    *RUN apt-get -y update*\
    *RUN apt-get install nano*\
    *CMD ["/bin/nano", "/tmp/notes"]*
  
    On Terminal :\
    *docker build -t nanoer.*\
    *docker run --rm -t nanoer*

##### 2 Dockerfile syntax

- FROM java:8 : first command of dockerfile
- MAINTAINER firstname lastname email
- RUN echo hello
- ADD run.sh /run.sh


##### 2 Multi-project Docker Files

Dockerfile : \
*FROM ubuntu:16.04 as builder*\
*RUN apt-get -y update*\
*RUN apt-get -y install curl*\
*RUN curl https://google.com | wc -c > google-size*\

*FROM alpine*\
*COPY --from=builder /google-size /google-size*\
*ENTRYPOINT echo google is this big; cat google-size*




