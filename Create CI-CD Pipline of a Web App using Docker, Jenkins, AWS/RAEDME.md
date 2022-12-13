
# Create CI/CD Pipline of a Web App using Docker, Jenkins, AWS

A brief description of what this project does and who it's for

project source: 

## CI/CD Pipline of a Web App using Docker, Jenkins, AWS

### Step1: Configure Docker
- First we need to launch an EC2 instance with Keypair
```
instanceType: t2.micro
AMI: "Ubuntu 22.04.1 LTS"
Security Group: 
22, SSH
8080,8001 Custom TCP
```
- Once our server is running, connect to your server via SSH by using your keypair.
- Then I will create directory whose directory name is ```project```.
- Then I will clone code using git. 
```
$ mkdir project
$ git clone https://github.com/anmolsharma40/django-todo.git

Cloning into 'django-todo'...

remote: Enumerating objects: 310, done.

remote: Counting objects: 100% (25/25), done.

remote: Compressing objects: 100% (20/20), done.

remote: Total 310 (delta 7), reused 17 (delta 5), pack-reused 285

Receiving objects: 100% (310/310), 390.05 KiB | 627.00 KiB/s, done.

Resolving deltas: 100% (158/158), done.

$ ls 
django-todo

$ cd django-todo/
$ ls
db.sqlite3  LICENSE  manage.py  nohup.out  README.md  staticfiles  todoApp  todos
```
- Then now install ```docker.io``` and ```create Dockerfile```  
- Then now build docker image and also run docker container.

```
$ sudo apt update -y
$ sudo apt install docker.io
$ vi Dockerfile
$ sudo docker build . -t todo-App
$ sudo docker images
REPOSITORY   TAG       IMAGE ID       CREATED              SIZE

todo-app     latest    db8ef40a36da   About a minute ago   985MB

$ docker run -p 8001:8001 todo-app 
Watching for file changes with StatReloader

System check identified some issues:



WARNINGS:

todos.Todo: (models.W042) Auto-created primary key used when not defining a primary key type, by default 'django.db.models.AutoField'.

	HINT: Configure the DEFAULT_AUTO_FIELD setting or the TodosConfig.default_auto_field attribute to point to a subclass of AutoField, e.g. 'django.db.models.BigAutoField'.



System check identified 1 issue (0 silenced).
```
#### Appendix
```
- Then You can access [Todo-App] Using Docker container.
- And You can ignore all [WARNINGS]. 

``` 

### Step2: Now I will  create jenkins images using docker

- Now pull a jenkins images using docker. Then create a jenkins conatianer .
```
$ sudo docker pull jenkins/jenkins
Using default tag: latest

latest: Pulling from jenkins/jenkins

a8ca11554fce: Pull complete 

9efd36b289e8: Pull complete 

9dd3ef156440: Pull complete 

192fc4950eb9: Pull complete 

ebd28cf99b9c: Pull complete 

478981d099c2: Pull complete 

f7540c723ecc: Pull complete 

9cb783ce129d: Pull complete 

bcf7cbe0c8aa: Pull complete 

41d482de573d: Pull complete 

308ec0046269: Pull complete 

cdf79498f5c3: Pull complete 

05e49f00c810: Pull complete 

Digest: sha256:7ca1388b6e2d293815f3030f99ddfaa4d76ffbf47c819a2afcdce3bcf89e6691

Status: Downloaded newer image for jenkins/jenkins:latest

docker.io/jenkins/jenkins:latest


$ sudo docker images
REPOSITORY        TAG       IMAGE ID       CREATED          SIZE

jenkins/jenkins   latest    51432d6486b3   5 days ago       470MB

$ sudo docker run -d -p 8080:8080  docker.io/jenkins/jenkins:latest


```

### Step3: Now Set up jenkins 

#### ![Jenkins](https://www.google.com/imgres?imgurl=https%3A%2F%2Fmiro.medium.com%2Fmax%2F1400%2F1*G42_3Tf55BfK2zhycvP2fA.png&imgrefurl=https%3A%2F%2Famir-raza.medium.com%2Fup-and-running-with-jenkins-b521c19a7b9a&tbnid=oTZTxA41Uimv7M&vet=12ahUKEwjUlqyWtvT7AhVcjNgFHbn5C_YQMygEegUIARC9AQ..i&docid=8MKM2kh7q-KegM&w=1171&h=810&q=unlock%20jenkins%20png%20image&ved=2ahUKEwjUlqyWtvT7AhVcjNgFHbn5C_YQMygEegUIARC9AQ)

- Now use this method you can see ```/var/jenkins_home/secrets/initialAdminPassword``` this path. 
- This path is show jenkins password without password jenkins is not open .
- If you run Jenkins inside docker as a detached container, you can use ```docker logs containerId``` to view the Jenkins logs.

#### ![Customize Jenkins](https://www.google.com/imgres?imgurl=https%3A%2F%2Fwww.learn-it-with-examples.com%2Fdevelopment%2Fcontinuous-integration-continuous-delivery%2Fjenkins%2Fpictures%2Funlock-jenkins%2F3-unlock-jenkins-login.png&imgrefurl=https%3A%2F%2Fwww.learn-it-with-examples.com%2Fdevelopment%2Fcontinuous-integration-continuous-delivery%2Fjenkins%2Funlock-jenkins.html&tbnid=24GGR0_J9fXm-M&vet=12ahUKEwjUlqyWtvT7AhVcjNgFHbn5C_YQMygDegUIARC7AQ..i&docid=ER9UM3RVbQhdcM&w=926&h=660&q=unlock%20jenkins%20png%20image&ved=2ahUKEwjUlqyWtvT7AhVcjNgFHbn5C_YQMygDegUIARC7AQ)
- Now click ```install suggested plugins``` on option.
- Then install all plugins automatically.

#### ![Create Admin User](https://www.google.com/url?sa=i&url=https%3A%2F%2Fserverfault.com%2Fquestions%2F1032542%2Fhow-to-create-jenkins-admin-user-later&psig=AOvVaw38wUg9eS9i8XTWYeVaEwgQ&ust=1670948021171000&source=images&cd=vfe&ved=0CBAQjRxqFwoTCOCL4su89PsCFQAAAAAdAAAAABAE)
- Fill all information carrefully.
- Then create jenkins account then you can access jenkins server```(port:8080)``` easily.

### Step4: Now Work on CD(continuous Deployment) part 
#### ![Setup agent](https://www.google.com/imgres?imgurl=https%3A%2F%2Fimages.ctfassets.net%2Fczwjnyf8a9ri%2F1oxTN2qGe0kcZM2HlqAtIV%2F0c2af2f9bfaa924e6612ead0a675435a%2Fjenkins-8.png&imgrefurl=https%3A%2F%2Fsaucelabs.com%2Fblog%2Fa-getting-started-guide-to-setting-up-jenkins&tbnid=BCz1iCxC8v0uGM&vet=12ahUKEwiilurb2_X7AhUXk9gFHe3FDYkQMygZegUIARDtAQ..i&docid=NYueTyygYgnkbM&w=867&h=457&q=set%20up%20agent%20jenkins%20png%20image&ved=2ahUKEwiilurb2_X7AhUXk9gFHe3FDYkQMygZegUIARDtAQ)
#### Note:- I will write only for changes in ```Jenkins Dashboard```. Who I will not write, So it will as it is.
##### Node name
``` TODO-APP DEV```

##### Type

```
✅Permanent Agent

Adds a plain, permanent agent to Jenkins. This is called "permanent" because Jenkins doesn’t provide higher level of integration with these agents, such as dynamic provisioning. Select this type if no other agent types apply — for example such as when you are adding a physical computer, virtual machines managed outside Jenkins, etc.

Then click Create option.
```
#### ![Node Dashboard](https://www.google.com/imgres?imgurl=https%3A%2F%2Fi.stack.imgur.com%2FyXiIc.png&imgrefurl=https%3A%2F%2Fstackoverflow.com%2Fquestions%2F70856556%2Fhow-to-use-wsl2-as-a-jenkins-agent-machine-from-jenkins-on-windows-10&tbnid=RHdcQLvZOZubdM&vet=12ahUKEwj8odir3_X7AhUPi9gFHQ97CRsQMygRegUIARDfAQ..i&docid=WgbEhAq0Kmu_XM&w=1261&h=925&q=node%20dashboard%20jenkins%20png%20image&ved=2ahUKEwj8odir3_X7AhUPi9gFHQ97CRsQMygRegUIARDfAQ)
##### Description
``` 
This is TODO-APP Pipline.
```
##### Remote root directory
```
/home/shyam/project
```
##### Lables
```
TODO-APP
```
##### ✅Use WebSocket

#### ![Craete a Job](https://www.google.com/imgres?imgurl=https%3A%2F%2Fwww.whizlabs.com%2Fblog%2Fwp-content%2Fuploads%2F2022%2F03%2FJenkins-start-page.png&imgrefurl=https%3A%2F%2Fwww.whizlabs.com%2Fblog%2Fci-cd-pipeline-inside-jenkins-tutorial%2F&tbnid=9RHvwqirDbO-yM&vet=12ahUKEwia8I_u4PX7AhWANbcAHbjdDGMQMyg7egQIARBL..i&docid=rzxS6IP08FuPeM&w=1423&h=755&q=welcome%20to%20jenkins%20png%20image&ved=2ahUKEwia8I_u4PX7AhWANbcAHbjdDGMQMyg7egQIARBL)
##### Enter an item name
```
TODO_APP
```
##### ✅Freestyle project
```
This is the central feature of Jenkins. Jenkins will build your project, combining any SCM with any build system, and this can be even used for something other than software build.
```
### General
#### Build Steps
####✅Add Build Step 

####✅Execute shell
```
cd /home/shyam/project/django-todo
docker build . -t todo-app
docker run -d -p 8001:8001 todo-app

```

```
 ✅Save
```
```
✅ ▶️Build Now
```
- Now you can see your code run easily without any error to follow all step . You can successfully ```continuous Deployment``` part build pipeline.

#### Step5: Now work on CI(Continuous integration) part

#### Note:- Now I will access CD Part . I will need github ```Personal access token```. So You can create Personal token help of github.
### ![Manage Jenkins](https://www.google.com/imgres?imgurl=https%3A%2F%2Fstatic.javatpoint.com%2Ftutorial%2Fjenkins%2Fimages%2Fjenkins-management2.png&imgrefurl=https%3A%2F%2Fwww.javatpoint.com%2Fjenkins-management&tbnid=uwLGbkgoawBfRM&vet=12ahUKEwj53aGr5_X7AhWviNgFHU31A4cQMygEegUIARDFAQ..i&docid=uKMCFnr76GEpLM&w=1428&h=758&q=manage%20jenkins%20png%20image&ved=2ahUKEwj53aGr5_X7AhWviNgFHU31A4cQMygEegUIARDFAQ)
##### System Configuration
```
✅ ⚙️Configure System
Configure global settings and paths.
```
### ![Configure System](https://www.google.com/imgres?imgurl=https%3A%2F%2Fstatic.packt-cdn.com%2Fproducts%2F9781788297943%2Fgraphics%2Fassets%2F2491acad-17c9-435e-9d44-12db1bc6b229.png&imgrefurl=https%3A%2F%2Fsubscription.packtpub.com%2Fbook%2Fcloud_and_networking%2F9781788297943%2F1%2Fch01lvl1sec10%2Fconfiguring-global-settings-in-jenkins&tbnid=hZmcjHdjuWbWEM&vet=12ahUKEwin7_SA6PX7AhV32HMBHZoOCkYQMygIegUIARC8AQ..i&docid=hlXloonEaaKCkM&w=1902&h=940&q=configure%20system%20in%20jenkins%20png%20image&ved=2ahUKEwin7_SA6PX7AhV32HMBHZoOCkYQMygIegUIARC8AQ)
✅ GitHub
```
GitHub Servers
✅ Add Github Server

```
```
Name 
 ## Github
Credentials
 ## ✅ Add(Jenkins)

# Jenkins Credentials Provider: Jenkins

Kind
 ## ✅secret text

 Secret
 ## YOU will do write Personal access tokne 

ID
 ## Jenkins-github

✅Add
```
#### ✅Save 


### Step5: Now Create CI/CD Pipline

#### ![Create a  Job](https://www.google.com/imgres?imgurl=https%3A%2F%2Fwww.whizlabs.com%2Fblog%2Fwp-content%2Fuploads%2F2022%2F03%2FJenkins-start-page.png&imgrefurl=https%3A%2F%2Fwww.whizlabs.com%2Fblog%2Fci-cd-pipeline-inside-jenkins-tutorial%2F&tbnid=9RHvwqirDbO-yM&vet=12ahUKEwiLhoSc6_X7AhXuzqACHcZ_C4gQMyg7egQIARBL..i&docid=rzxS6IP08FuPeM&w=1423&h=755&q=welcome%20to%20jenkins%20png%20image&ved=2ahUKEwiLhoSc6_X7AhXuzqACHcZ_C4gQMyg7egQIARBL)
### Note:- Just like same did you like above.

#### Source Code Management
```
✅Git
 
Repository URL
 ## https://github.com/anmolsharma40/django-todo

Branches to build
 ## */develop
```

#### Build Steps
####✅Add Build Step 

####✅Execute shell
```
sudo docker build . -t todo-App
sudo docker run -d -p 8001:8001 todo-App
```
### ✅Save

### ▶️Build

# Now CI/CD pipeline has been created, My project is over. And I hope if you follow all these Steps then you will not face any problem.






