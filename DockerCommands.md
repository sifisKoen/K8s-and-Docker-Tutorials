Docker Commands
================


Run a container:
```
docker run ( container name )
```

excamples :
```
docker run busybox
```
```
docker run hello-world
```
```
docker run redis
```

See all running containers:
``` 
docker ps
```
| CONTAINER ID | IMAGE  | COMMAND                | CREATED        | STATUS       | PORTS     |  NAMES            |
| -------------|:------:|:----------------------:|:--------------:|:------------:| :--------:| :----------------:|
| d285cd302ccb | redis  | "docker-entrypoint.s…" | 6 seconds ago  | Up 2 seconds | 6379/tcp  | boring_easley     |


If you do not have any containers to run it will be like:

| CONTAINER ID | IMAGE  | COMMAND                | CREATED        | STATUS       | PORTS     |  NAMES            |
| -------------|:------:|:----------------------:|:--------------:|:------------:| :--------:| :----------------:|



Now if you want to run a command inside to your runing container you need :

1 )
```
docker ps
```
see the Container ID for excample --> ( d285cd302ccb )

2 )
```
docker exec #command
```
 -i -t < your Containes ID  > the command you want to execute

-it        d285cd302ccb                  redis-cli / ls / ls -l

The command:
```
docker exec -it d285cd302ccb  redis-cli
```

How ever it's depends on what Container you want to run a command.
For example on redis you can not run ls or echo.
But on busybox you can run ls -l or mkdir or what command you want.


3 ) If you want to go in to containers environment:

```
docker exec -it 39819566116e sh
```
Ths command is like before but now we include the sh

Now you are into the containers terminal.
Is an linux environment.
So you can exec every commmand you exec on linux.

Like :
```TERMINAL
ls / ls -l / echo "" / cat / cd / mkdir ect,ect.....
```

# Build your oun image:


1 )

 Create a file
```
mkdir xxxx
```
2 )
```
cd xxxx
```
3 )

 Cteate an file with name Dockerfile

4 )

 Use your text edditor
```
vim Dockerfile
```
5 )

Now lets build our first docker image:
Put all of these into your docker file --> Dockerfile
```Dockerfile
# Use an existing docker image as a base
FROM alpine



# Download and insall a dependency
RUN apk add --update redis



# Tell the image what to do when is starts
# as a container
CMD ["redis-server"]
```
It will run the redis server but it is a first step.

6 ) 

 Build docker image now.
Exec the command: 
``` 
 docker build .
```
===========================================
```
ending build context to Docker daemon  2.048kB
Step 1/3 : FROM alpine
latest: Pulling from library/alpine
9d48c3bd43c5: Pull complete 
Digest: sha256:72c42ed48c3a2db31b7dafe17d275b634664a708d901ec9fd57b1529280f01fb
Status: Downloaded newer image for alpine:latest
 ---> 961769676411
Step 2/3 : RUN apk add --update redis
 ---> Running in 8d4585b5a560
fetch http://dl-cdn.alpinelinux.org/alpine/v3.10/main/x86_64/APKINDEX.tar.gz
fetch http://dl-cdn.alpinelinux.org/alpine/v3.10/community/x86_64/APKINDEX.tar.gz
(1/1) Installing redis (5.0.5-r0)
Executing redis-5.0.5-r0.pre-install
Executing redis-5.0.5-r0.post-install
Executing busybox-1.30.1-r2.trigger
OK: 7 MiB in 15 packages
Removing intermediate container 8d4585b5a560
 ---> 2c2d15302475
Step 3/3 : CMD ["redis-server"]
 ---> Running in 2335c50f18d3
Removing intermediate container 2335c50f18d3
 ---> 490e8c783419
Successfully built 490e8c783419
```
=========================================

Dockerfile --> Docker Client --> Docker Server --> Usable Image!

Steps:

 - In step 1/3
   You insall the apline. As our first image.
   
 - In step 2/3
    1. Create a container from the first image -->  8d4585b5a560
        ( Running in 8d4585b5a560 )
    2. Install redisinside the container
    3. Remove the container 8d4585b5a560 and in the first image install the new changes 
        into a new name image 2c2d15302475.
        So now we have a new image with apline and redis installed.
 
 - In stap 3/3
    1. Create a new container 2335c50f18d3 in these container will specify
        which will be the Startup Command for our Image --> redis-server  
    2. Remove the container 2335c50f18d3 and create a new one image --> 490e8c783419
        where will be update the older image with the new Startup Command.

7 )
- Run your image.
 1. Go at the last line fron the output of the last command.
 2. See the image ID.
 3. Copy.
 4. Run the command.
     ```
     docker run < your image ID >
     ```
Example:
 
 look the last line on 6th step. ( Successfully built 490e8c783419 )
 ```
 docker run 490e8c783419
 ```
=================================================================================

Now if you want to put one more code line in your image:

```
#Use an existing docker image as a base

FROM alpine



# Download and insall a dependency
RUN apk add --update gcc  #new line
RUN apk add --update redis


# Tell the image what to do when is starts
# as a container
CMD ["redis-server"]
```

And now you go to build again the image with command --> ```docker build .```.
You will see tha the docker will understand that you don't make any
changees but tha only change you made is : < ```RUN apk add --update gcc``` > command is the new
command in your code.

=================================================================================

Now as we say before to run an image you need the image ID.
Wich is like --> < 490e8c783419 >.
This is difficult to remeber. So we can give name to our image or tag it.
The command is:

                           < Tags the image >
        docker build -t myfirstdocker/redis:latest .


                -t < Your DOcker ID > / < Repo / Project name > : < Version >







