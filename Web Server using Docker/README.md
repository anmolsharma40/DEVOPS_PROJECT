
# Web Server using Docker

A brief description of what this project does and who it's for


## Apache Server through a Dockerfile

We need to follow the below steps to set up an Apache Server through a Dockerfile:

- Create a directory for all the files related to Apache set up
- Create a Dockerfile
- Build an image over the Dockerfile and tag the same for convenience
- Run the Apache server as a container

### Step 1: Create a directory for Apache server files
- At first, we make use of the mkdir command to create a directory specifically for all the Apache-related files.
```
$ mkdir apache_folder
```
### Step 2: Building a Dockerfile
- Having created a folder, now we go ahead and create a Dockerfile within that folder with the vi editor:
```
$ vi Dockerfile
```
- As soon as we execute the previous command, a vi editor opens. Paste the following content in the Dockerfile:
```
$ FROM ubuntu 
$ RUN apt update 
$ RUN apt install -y apache2 
$ RUN apt install -y apache2-utils 
$ RUN apt clean 
$ EXPOSE 80
$ CMD [“apache2ctl”, “-D”, “FOREGROUND”]
```
- To exit the editor, press ESC then :<wq! then Enter.

### Step 3: Tag and build the Docker image
- Now, we build the Dockerfile using the docker build command. Within which, we tag the image to be created as 1.0 and give a customized name to our image (i.e., apache_image).
```
$ docker build -t apache_image:1.0 .
```
- Once the image has been built, we should check for the presence of the image using docker images command.

- The docker images command gives us a list of all the images that are built or pulled from any public/private registry.
```
$ docker images
REPOSITORY                                                      TAG                 IMAGE ID            CREATED             SIZE
apache_image                                                     1.0                 a738dbef66ef        15 seconds ago      133MB
```
### Step 4: Run the Docker image as a container
Once the image has been built, run the image as a container locally:
 - We run the container in detached mode so that it runs continuously in the background. Include -d in the docker run command.
 - In order to host the Apache server, we provide port 80 (HTTP) for the same. Make use of -p 80:80 to have the server running on localhost.
Thus, the docker run command also takes the image along with the associated tag as input to run it as a container.
```
$ docker run --name myapache -d -p 80:80 apache_image:1.0
```
```
$ docker ps
  CONTAINER ID        IMAGE               COMMAND                  CREATED             STATUS              PORTS                NAMES
  443848c30b74        apache_image:1.0     "/docker-entrypoint.…"   7 seconds ago       Up 6 seconds        0.0.0.0:80->80/tcp   myapache
```
### Step 5: Review the online presence of Apache Server
In order to test the presence of Apache server on the system, visit any local browser and type localhost:

### ![Apache2](https://journaldev.nyc3.digitaloceanspaces.com/2021/05/image.png)

### Conclusion
     - By this, we have come to the end of this topic. Feel free to comment in case you come across any questions.

     - For more such posts related to Docker, Stay tuned with us.

     - Do let us know your experience in setting up the Apache Server on your workstations in the comment box.

Till then, Happy Learning!
