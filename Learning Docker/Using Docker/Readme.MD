###### Commands :

###### 1
In Docker, programs run in containers, which are comprised of images.

- *docker images* : List of all the images
- *docket ps* : running containers
- *docker run -ti hello-world:latest* : if image " hello-world is not there, then it will download and open.
- *docket ps -l* : last container
-  "-ti" means terminal interactive



###### 2
- *docket ps -a* : all container (running / exited)
- *docker images*  : to check all the images
- *docker run -ti < Repository name/ Image ID> < bash>* : make changes to the container
- *docker commit < Container ID / NAMES> < Image Name>* : your changes will be saved on that container




###### 3. Run processes in containers
- *docker rm < container ID/ NAMES>* : removes the container
- *docker run --rm -ti < container Name >* :  --rm will automatically deletes the container after it exits.
- *docker run --rm -ti ubuntu sleep 5* : start -> run for 5 sec-> exits -> delete
- *docker run -ti ubuntu bash -c "sleep 3; echo all done!"*

- docker run -d -ti ubuntu bash : Detach the container and it will run in a background.
- docker attach < container ID/ NAMES > : it will take it you to that container.
- control-p + control-q : will detach the container and you can again use that container by attaching it again.
- docker exec -ti < container Name / ID > < bash > : starts another process in an existing container. Remember if original container exxits then child process will also dies.This command adds another process to a running container, and is good for debugging.

###### 4. Manage containers

- A Docker container continues to run until the process that started it exits
- *docker run --name example -d ubuntu bash -c "lose /etc/password"* : create a container name "example" and run the command
- *docker logs example* : gives you the logs for that container
- *docker kill < container_name>* : stops the container
- *docker rm < container_name>* : remove the container

- Resource contraints using --memory / --cpu-shares / --cpu-quota

###### Lessons from the field

- Don't let your container fetch dependencies when they start
- don't leave important things in unnamed stopped container.

###### 5. Exposing ports

- *docker run --rm -ti -p 45678:45678 -p 45679:45679 --name echo-server ubuntu:14.04 bash*  

- *docker port  echo-server*

- Terminal 1 : *nc -lp 45678 | nc -lp 45679*
   lp : listen port (ulp for udp)
   nc : netcat
   In example , 45678 : input port and 45679 : output port


- Terminal 2 : *docker run --rm -ti ubuntu:14.04 bash*
               *nc host.docker.internal 45678*

- Terminal 3 : *docker run --rm -ti ubuntu:14.04 bash*
               *nc host.docker.internal 45679*

Now whatever you send in termial will shown in terminal 3

* 45678:---- if you don't write port seen from outside then docker will select some random port and using " docker port < name-of-the-port>" 

- docker run -p outside-port:inside-port/protocol (tcp/udp)

###### 6. Container Networking

- bridge : This is the default network driver; if you do not specify a network, this is the type you are creating.
- *docker network ls* :  exisiting networks 
- *docker network create < name >* : create a network

- Terminal 1 : *docker network create learning* 

- Terminal 2 : *docker run --rm -ti --net learning --name catserver ubuntu:14.04 bash*
               *ping catserver*
- Terminal 3 : *docker run --rm -ti --net learning --name dogserver ubuntu:14.04 bash*
               *nc -lp 1234*
- Now T2 & T3 will act as a chat terminal which works in both the ways.

###### 7. Legacy Linking

- Legacy linking works in one direction
- Terminal 1 : *docker run --rm -ti -e SECRET=secretkey --name catserver ubuntu:14.04 bash*
- Terminal 2 : *docker run --rm -ti --link catserver --name dogserver ubuntu:14.04 bash*
- now T2 can see all the env variables of T1 but vice-versa is not possible.


###### 8. Images

- docker pull (docker run automaticall calls pull)
- docker push
- docket rmi < image-name:tag / image-id> : clean up unused images

###### 9. Volumes

- docker volumes can be shared from one conatiner to another but once all the associated container are exited and volume will be lost.

- *docker run -ti volumes-from < already-created-container-name> ubuntu bash* : inherit the volumes from existed container.

###### 10. Docker Registries

- *docker login* : used to login into docker

