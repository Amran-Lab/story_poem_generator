# Documentation
---
# Poem & Story Generator

This is for the second project set by QA

## Table of Contents

1. [Project Brief](#project-brief)
    + [Proposal](#proposal)
    + [Wireframes](#wireframes)
2. [Trello Board](#trello-board)
    + [Start Point](#start-board)
    + [Rolling Changes](#rolling-changes)
    + [End Point](#end-point)
3. [Risk Assessment](#risk-assessment)
4. [Project Architecture](#project-architecture)
    + [Architecture Diagram](#architecture-diagram)
    + [Networking](#networking)
    + [Deployment](#deployment)
    + [Issues Encountered](#issues-encountered)
    + [Toolset](#toolset)
6. [Testing](#testing)
    + [Pytest Testing](#pytest)
7. [Retrospective](#retrospective)
8. [How to run the webapp?](#install)
9. [Author](#authors)

## Project Brief

To create an app using:
+ Software Development with Python
+ CI/CD using Jenkins, Ansible pipeline (Seperate Cloud Services)
+ Nginx as reverse proxy
+ Managed MySQL database on the cloud
+ Docker and Orchestration using Docker-Swarm, Split app into micro-services in the cloud with built in redundancies such as Load-Balancing behind Nginx. (One Manager node, One Worker Node in Swarm)
+ Use Ansible to set up all VM's (Installing Dependencies and applications) using Playbook
+ Use Jenkins to set up multiple stages in the pipeline; Staging, Testing, Building and Deployment and implement a Git Webhook as a trigger
+ Implement Cloud Fundamentals such as test driven development, Continuous Integration and deployment, and a SCRUM based methodology
+ Micro-service oriented architecture composed of at least 4 services that work together and services 2,3,4 should be easily interchangeable using versioning on Dockerhub.

### Proposal

I decided to create a Story & Poem Generator with a Microservice architecture. My application will have 4 services not including Jenkins, Managed MYSQL db, Nginx server, Ansible Server and Docker Swarm.
+ Service 1 of my project is essentially the front-end where the stories and poems are displayed.
+ Service 2 and 3 will be services that generate the random theme and character name.
+ Service 4 is the back-end that decides between whether the user gets a "story" or a "poem".
+ I will be using Ansible as a way to stage all my environments, prepare them to run my services and deploy the docker swarm.
+ I will use Jenkins to build a testing and deployment pipeline using Jenkinsfile that will be triggered using Git/Github webhook.
+ Nginx will be used as a Reverse Proxy to load balance all my services running on Docker Swarm.
+ Have full CRUD functionality

### Wireframes

<img src="https://i.imgur.com/pWyR2jp.png" title="wireframes" />

+ My initial idea was to have a simple button so when the user pressed it, it displayed a story of a random theme with random character names substituted in it. In other words, service two picks out a theme at a random from a list of three and service three picks out a male and female character name from a list of fourteen, for which service four puts it all together and sends it back to service one. 

+ To make it more user friendly/interactive, I changed it so that the user gets to choose the theme and character name they want by adding user input. This shortened the character name list to three from fourteen. 

+ In the end, instead of having service four take a lot in with sending a whole story over to service one, I thought I could just store the literacy pieces in service one and have service four just choose between poem and story depending on service two and three. 

+ After implementing crud, my website looked as below:

<img width="350" src="https://i.imgur.com/2uFXicC.png" title="first page" />  <img width="350" src="https://i.imgur.com/OoZ40Zm.png" title="2nd page" />

## Asana Board

I used a kanban board on Asana to manage my workflow during the project. Agile methodology was implemented in line with the brief, in terms of product and sprint backlog, although due to the individual nature of the project, no scrum working practices were applied. The board was set up with reference to potential user stories. I split the sprints into smaller tasks in order to keep the requirements of the project spec clear.

Due to this setup sprints could be passed through development and, if required, testing, before being assigned to Done. Once sprints generated by a backlogged Task have been completed, the task itself can be marked Done. Functionalities that requirement more time and weren't neccessary to implement are given over to the newly added Dropped ideas column.

### Start Point

<img width="250" src="https://i.imgur.com/KQNZeaX.jpg" title="moscow key" /> 

<img width="200" src="https://i.imgur.com/VTKAvln.jpg" title="product backlog1" /> <img width="200" src="https://i.imgur.com/6khWR1B.jpg" title="product backlog2" /> <img width="200" src="https://i.imgur.com/SmZF0a4.jpg" title="product backlog3" /> <img width="200" src="https://i.imgur.com/jxzMAkA.jpg" title="product backlog4" />


<img src="https://i.imgur.com/JZuSiNO.png" title="sprint1" />

At the start of the project, I focused on the five tasks most easily completable in the first week of training: Starting the Kanban board [itself](https://app.asana.com/0/1169906447683321/board), starting this documentation, instituting a github repository for the project, which can be found [here](https://github.com/thenu97/story_poem_generator), initialising the risk assessment for the project in line with my initial understanding, and researching Docker covered in lesson as we went along.

### Rolling Changes

+ The first major change to the Kanban board to changing my idea from a user pressing a button to selecting data; theme and character names. This meant I had to change my html code and find a way to communicate the user input to service two and three. Then communicate this info to service four at the same time. I spent most of my time researching flask before_requests and after_requests, and using global variables to store data coming in at different times. 

+ As I was using global variables to solve my issue of post requests coming in at different times, I was faced with another issue when replicating the container for docker swarm. The global variables were container specific so when I was replicating it, the global variables reset to empty. To solve this, I had three options: to replicate the services utilising global variables once and document the issue, research into docker volumes and see if there's a way of replicating the containers based on the volumes of the original container, or re-design my entire application to ensure it wasn't using global variables at all. In the end, I went with the third option and gave myself a day to re-design the application and if it wasn't completed within that time-frame to result in the first resolution. 

+ Reaching the end of week two, I had an application that didn't use global variables and had also found a way to programme Ansible to set the environment (downloading docker) assign worker/manager nodes without having to ssh into each virtual machine and manually doing it ourselves. I also researched into what I had to do for testing so added that onto to Yet to start on the kanban board. 

+ SonarQube was not a requirement to implement so that became a dropped idea. 

+ Added CRUD functionality. Subsequently, changed testing to fit crud functionality and to get my testing above 50% mark. 

### End Point

<img src="https://i.imgur.com/Zqspd1D.png" title="ending" />


## Risk Assessment

|Risk No.|Risk|Effect|Likelihood|Serverity|Importance|Mitigate|
|---|---|---|---|---|---|---|
|1|Problems with developing 4 services.|Failed project as this is a needed requirement|3|5|15|Do more research into mircoservices and how systems involved work together.|
|2|Overrunning on GCP free data limits.|An instance is left running, or an account breach enables the resources on the account to be drained.|1|5|5|Continue monitoring GCP usage. Copy databases offline as final backup.|
|3|Problems with Python/Flask|Failed project as this is needed for the front end and backend of my application.|1|2|2|Practise Python through continuous challenges on Codewars.|
|4|Problems with Jenkins|Not being able to CI/CD|2|3|6|Do more research into using Jenkins with Docker Swarm.|
|5|Using Ansible Playbook|The project requires this but due to not using ansible before, this holds a great risk.|3|4|12|Study how Ansible is used through documentation provided and asked my trainer for resources.|
|6|Docker|Required by the project but I haven't used this tool before so it holds high risks.|2|3|6|Research and docker documentation.|


<img src="https://i.imgur.com/I0HiOw2.png" title="risk matrix" />


|Risk No.|Risk|Solution|
|---|---|---|
|1|The brief being too hard to decipher|Asked my peers, and emailed my manager to join our daily standups who explained the brief in more detail.|
|2|Developing mircoservices as it was a new concept to me|I put in hours of research to figure out how everything interlinked.|
|3|GCP free credit running out|I stopped an instance when it wasn't being used and monitored the rate of credit drop.|
|4|Facing issues with python/flask|Ensured I was doing at least 3 challenges a day to keep my python knowledge on pur.|
|5|Jenkins automated build|I researched into how Docker Swarm tied in with Jenkins and drew a pipeline that help me grasp the idea better.|
|6|Ansible - worker nodes|Researched into it and I was eventually able to support my colleagues with a solution that managed docker clusters.|
|7|Docker Swarm|Docker wasn't as hard as I expected to understand. Continuous practise helped a lot.|
|8|GCP service was throwing errors when running website|Recorded the deployment process so I can demonstrate it during presentation.|


## Project Architecture

### Overall Architecture

<img src="https://i.imgur.com/RkYpe6V.png" title="architecture" />

+ I essentially have three virtual machines: one master, one manager, one worker. My master has only Jenkins and ansible installed on it. 
+ Jenkins first uses a pipeline to test if docker installation, docker-swarm deployment works within the master node. 
+ If that passes, it invokes Ansible. 
+ Ansible SSHs into the manager and worker node to set the environment and initiate swarm manager and ensures the other node joins as worker. 
+ It then SSHs into just the manager node to deploy the stack.
+ One of the fundamental of swarm is that it shares the number of containers among the nodes. [5 containers (including NGINX) with 3 replicas]
+ NGINX then comes in as a reverse proxy and directs any service coming in on port 5000 to port 80. 

### Networking

<img src="https://i.imgur.com/YoNkeW8.png" title="communication" />

+ The services are running on the same docker overlay network.  
+ This enables them to send and receive post/get requests.

### Deployment

<img src="https://i.imgur.com/i3cK01G.jpg" title="deployment process" />

+ Code changed on VSC by developers and pushed up to GitHub
+ GitHub uses webhooks to initiate build in Jenkins 
+ Jenkins then goes ahead and tests the installation process and URL locally 
+ Initiates Ansible to set the environment, assign nodes and deploy swarm


### Toolset

<img src="https://i.imgur.com/wl13iqk.png" title="toolset" />

+ GCP Instances (Cloud Provider)
    + Virtual machines are based on computer architectures and provide functionality of a physical computer. Their implementations may involve specialized hardware, software, or a combination.
    + MySQL for data persistence.

+ GitHub - Version Control
    + Feature-branch model. I had 3 branches: master, dev, jenkins. 
    + Master branch was the clean version where the commits and changes were kept very minial
    + Dev branch was for experimentation with docker
    + Jenkins branch was for playing aroung with my jenkins pipeline. 
    + Jenkins branch was always ahead of the other two. Dev was ahead of master.
    
<img src="https://i.imgur.com/0tbS59L.png" title="master" /> <img src="https://i.imgur.com/NAzsAZG.png" title="dev" /> <img src="https://i.imgur.com/66hMzyn.png" title="jenkins" />

+ GitHub Webhook
    + Every push on jenkins branch initiated jenkins build.  

+ Jenkins Server (CI tool)
    + Pipeline with 7 stages:
        + Test Docker Install
        + Test Docker Swarm
        + Install Ansible
        + Envirnoment Setting (via Ansible)
        + Nodes Assigning (via Ansible)
        + Deploy Swarm (via Ansible)
        + Testing
    + Pipeline coded in Groovy & Shell
    + Details about Jenkins can be found [here](https://jenkins.io/doc/)

+ Testing in Pytest using the Coverage module.

+ Ansible in YAML (CD & Orchestration tool)
    + Automation Engine
    + Envirnoment Setting (via Ansible)
    + Nodes Assigning (via Ansible)
    + Deploy Swarm (via Ansible)
    + Testing
    + Details about Ansible can be found [here](https://docs.ansible.com/)

+ Docker/Docker Swarm (CD tool)
    + Containerisation instrument
    + Uses images from Docker Hub to create containers
    + Uses networks to communicate within containers
    + Dockerfiles are used to create images
    + Docker-compose are files written in YAML for the creation and activation of containers.
    + Details about Jenkins can be found [here](https://docs.docker.com/)

+ NGINX (Reverse proxy)
    + Acts as a "traffic cop"
    + Compresses inbound & outbound data
    + Details about NGINX can be found [here](https://nginx.org/en/docs/)


## Testing

### Pytest

<img src="https://i.imgur.com/hll5I14.jpg" title="coverage html" />

+ The coverage was 56% because I didn't test everything in app.py 
    + It was 16% for app.py because I imported a route name from service 1
+ URL tests were 100% because the application was up and running 
+ Database tests were 100%

<img src="https://i.imgur.com/cq6ZbKK.png" title="source: imgur.com" />

+ 12 tests running, 5 database & 7 url.
+ After I added crud, my coverage went down to 52%
    + This is because there are now more statements in service one that aren't being tested as I haven't implemented selenium. 
+ Remained above 50% as I added more database and url testing

## Retrospective

### What Went Well?
+ Completed project within timeframe and most of it being above spec.
+ Involving multiple VMs achieved
+ Ansible and Docker were very interesting tools to learn

### Issues Encountered

+ At the end of week two, I had to change my entire project around to ensure they were not using global variables. Global variables did not go well with the replication of containers. I did want to look into the properties of volumes but I wanted to structure my application a little better.
+ Due to not having previous experience with Ansible, it was hard to understand how to assign tasks.
+ Automated build in Jenkins wouldn't start at every git push. 
+ Constantly changing IP addresses pf VM instances on GCP due to them not being static. 
+ SonarQube not wanting to run although I pulled down the latest image on dockerhub. 
+ The automated docker builds took their time as it was a free service. 

### Future Improvements

<img width="500" src="https://i.imgur.com/MH7lxA1.png" title="source: imgur.com" />

+ Selenium to get coverage above 70%
+ More theme options for the user
+ Add music to match the theme as another service
+ Front-end design implementation
+ Better back-end coding


## How to run the webapp?

### Requirements:
- Visual Studio Code
    - download [here](https://code.visualstudio.com/download)
- Git Bash
    - download [here](https://gitforwindows.org/)
- Google Cloud Platform account (google account)
    - create [here](https://accounts.google.com/SignUp?continue=https%3A%2F%2Fwww.google.co.uk%2F&hl=en&gmb=exp&biz=false)

### Installation Guide
1. Using your google account, login into Google Cloud Platform
2. Create 2 (ideally 3) Compute Engine Instances (Ubuntu 18.04): master, manager & worker (for 3) or manager & worker (for 2). 
    - Firewalls:
        - Allow HTTP traffic in all
        - Port 8080 needs to be opened on manager (and master if 3)
3. Create 1 MySQL SQL instance.
    - Firewalls (Connectivity):
        - Public IP could be 0.0.0.0/0
        - For more security, I highly recommend adding your home IP address instead.
4. Open Git Bash and run the command ```ssh-keygen```, this will prompt you to name the key files (I stick to the basic side and call it: id_rsa)
5. In file-explorer, find id_rsa & id_rsa.pub and put them into a folder called .ssh
6. Open id_rsa.pub with notepad. Copy and paste all of it into the ssh key part of your compute engine instances. This is how you'll ssh into each instance from one another.
7. Add a config file on your Visual Studio Code in the format of: 
    ```
    Host manager #name it anything, I called it manager
        HostName XX.XXX.XX.XX #ip address of the instance you want connected to VSC 
        User Admin #name of the user you generated the key as
        IdentityFile ~/.ssh/id_rsa' #path to private key
8. Once you've connected to your instances via Visual Studio, clone down the jenkins branch into your manager (and master, if using 3 instances) instance using the command ```git clone -b jenkins https://github.com/thenu97/story_poem_generator.git```
9. Download Jenkins onto manager (if using 3, then only download it onto master). 
    - Instructions can found [here](https://www.guru99.com/download-install-jenkins.html)
    - For jenkins, you'll need java jdk. This can be installed by running the following commands:
    ```
        sudo apt-get update
        sudo apt-get install openjdk-8-jdk
        java -version #to check the version
10. Open jenkins by running the ip address on port 8080 on the browser and set up a new pipeline by attaching it to the forked ver of this project on your repo.
11. You can also get up webhooks between github and jenkins
    - More info provided [here](https://dzone.com/articles/adding-a-github-webhook-in-your-jenkins-pipeline)
10. Download Ansible onto manager (if using 3, then only download it onto master).
    - Instructions can found [here](https://docs.ansible.com/ansible/latest/installation_guide/intro_installation.html)
11. Now for the security part, to help not expose any credentials:
    - For SQL database details:
        - Docker-compose:
            - Create a .env file in the same directory of the project. The format:
            ```    
                MYSQL_HOST=XX.XX.XXX.XXX (ip address)
                MYSQL_USER=
                MYSQL_PASSWORD=
                MYSQL_DB=
        - Tests:
            - Create a .bashrc within the jenkins server:
                ```
                sudo su jenkins
                cd ~
                sudo vim .bashrc
            - Add the following to the .bashrc file:
                ```
                export MYSQL_HOST=XX.XX.XXX.XXX (ip address)
                export MYSQL_USER=
                export MYSQL_PASSWORD=
                export MYSQL_DB=
    - For IP addresses on ansible inventory.config file:
        - On the node with jenkins installed in it, run the following commands:
            ```
            sudo su jenkins #going in as a jenkins user
            cd /etc
            sudo vim hosts
        - Within the file hosts, under localhost add:
            ```
            XX.XX.XXX.XXX worker-node #you can name it anything
            XX.XX.XXX.XXX manager-node #if 3rd VM is used.


## Authors

Thenuja Viknarajah