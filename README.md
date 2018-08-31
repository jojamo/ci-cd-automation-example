# DevOps Example
Example automation using ant, docker &amp; jenkins.
Designed for use with Unix systems.

# This project covers:
- Building a docker image locally and testing it (Local address will display 'Hello World').
- Pushing code to github, triggering a jenkins build which builds & pushes a docker image to dockerhub.
- Using ant to automate areas of the project (see: build.xml)

# Project includes
- dockerfile example
- jenkinsfile example
- ant script example

# Requirments:
Common Software:
- sudo apt-get install \
    apt-transport-https \
    ca-certificates \
    curl \
    software-properties-common

Run Update:
sudo apt-get update

Install JDK:
- sudo apt-get update && sudo apt-get upgrade
- apt-get install default-jdk

Install Ant:
- sudo apt-get install ant

Install Docker-ce
- sudo apt-get install docker-ce

Install Docker-compose
- sudo apt-get install docker-compose

Jenkins .war & cli is included, but alternatively if you would like to download yourself, they can be found at:
- https://jenkins.io/download/
- https://wiki.jenkins.io/display/JENKINS/Jenkins+CLI

Dockerhub account
- https://hub.docker.com/
- Create repo via dockerhub

# Auto project setup using ant (launch jenkins, build image, etc)
- ant jenkins
- ant build
- ant compose
- ant stop

# Configuring Jenkins:
Modify this line in the Jenkinsfile to the repo you setup:
        app = docker.build("jjmorris/example")

Make sure jenkins is running

Add jenkins to the root group so it has permissions (will fail when building otherwise)
- sudo usermod -a -G root jenkins

After Jenkins has been launched, go to the local address hosting it & configure it
- http://<computer-name>:8080

To start a jenkins build automatically you need to set up the webhooks between github and jenkins. 
Also because jenkins is running locally, github cannot send a signal to it to trigger a build. 

So you have to use a proxy: (8080 is the port running jenkins)
- ssh -R 80:localhost:8080 serveo.net

After running the command above, any push you make to github will trigger a build to jenkins if you have set up everything correctly.

Extras:
Run Jenkins with logs:
- sudo nohup java -jar jenkins.war > $LOGFILE 2>&1

To enter jenins cli:
- java -jar jenkins-cli.jar -s http://<computer-name>:8080/ help

# Example of use
- ant jenkins : This will start a local jenkins instance
- ant build   : This will build & compose a docker image
- In your browser, check everything has worked by navigating to: http://127.0.0.1:8000 
- Configure jenkins with a webhook to your github
- Edit 'main.json' to show a diffrent message
- git add . 
- git commit -m "your message"
- git push
- Jenkins should now be building a new image & pushing the image to your dockerhub repo

# Areas to expand
I have left an area in the jenkins file where unit tests can be called & run during the build process, I may add these in the future, but adding your own if you have any should work.

A docker database container could also be spun up and could display data to the web docker container. I may also add this in the future, there are plenty of resources online if you wish to do so yourself.

Add to the ant script to make it pull an already built docker image from your repo

Need to make this cleaner to read!
