# Jenkins
## Day1 answered in day1/day1-jenkins.pdf
## Day2 brief commands 
### configure jenkins image to run docker commands on your hos docker daemon
```
# Use the official Jenkins image as the base image
FROM jenkins/jenkins:lts

# Install the Docker CE in the Jenkins container
USER root
RUN apt-get update && apt-get install -y apt-transport-https ca-certificates curl gnupg-agent software-properties-common
RUN curl -fsSL https://download.docker.com/linux/debian/gpg | apt-key add -
RUN add-apt-repository \
    "deb [arch=amd64] https://download.docker.com/linux/debian \
    $(lsb_release -cs) \
    stable"
RUN apt-get update && apt-get install -y docker-ce

RUN usermod -aG docker jenkins

```
### create CI/CD for this repo https://github.com/mahmoud254/jenkins_nodejs_example.git
<div>
<img src="https://github.com/RaniiaAshraf/Jenkins/blob/main/day2/pics/new.png" width="800" height"500">
<img src="https://github.com/RaniiaAshraf/Jenkins/blob/main/day2/pics/5.png" width="800" height"500">
</div>

### create docker file to build image for jenkins slave
```
FROM ubuntu

USER root

RUN mkdir -p jenkins_home
RUN chmod 777 jenkins_home

ENV DEBIAN_FRONTEND noninteractive
ENV TZ=Africa/Cairo

RUN apt-get update

RUN apt-get install -y tzdata

RUN apt-get install -y openjdk-11-jdk

RUN apt-get install -y openssh-server


# Install dependencies required to install Docker
RUN apt-get install -y apt-transport-https ca-certificates curl gnupg-agent software-properties-common

# Add the Docker GPG key
RUN curl -fsSL https://download.docker.com/linux/ubuntu/gpg | apt-key add -

# Add the Docker repository
RUN add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable"

# Update the package repository again
RUN apt-get update

# Install Docker
RUN apt-get install -y docker-ce docker-ce-cli containerd.io

# Verify the Docker installation
RUN docker --version

RUN useradd -ms /bin/bash jenkins
RUN usermod -aG docker jenkins

WORKDIR jenkins_home

CMD [ "/bin/bash" ]
```
### create container from this image and configure ssh
```
sudo docker build -t jenkins-image:v1 .

```
```
sudo docker run -d --name jenkins-slave-container -it jenkins-image:v1
```
```
ssh-keygen
```
### from jenkins maste create new node with the slave container
<div>
<img src="https://github.com/RaniiaAshraf/Jenkins/blob/main/day2/pics/10.png" width="800" height"500">
</div>
#### integrate slack with jenkins
<div>
<img src="https://github.com/RaniiaAshraf/Jenkins/blob/main/day2/pics/12.png" width="800" height"500">
</div>

### send slack message when stage in your pipeline is successful
<div>
<img src="https://github.com/RaniiaAshraf/Jenkins/blob/main/day2/pics/15.png" width="800" height"500">
<img src="https://github.com/RaniiaAshraf/Jenkins/blob/main/day2/pics/14.png" width="800" height"500">
</div>

### install audit logs plugin and test it
<div>
<img src="https://github.com/RaniiaAshraf/Jenkins/blob/main/day2/pics/Screenshot%20from%202023-02-09%2020-29-08.png" width="800" height"500">
<img src="https://github.com/RaniiaAshraf/Jenkins/blob/main/day2/pics/18.png" width="800" height"500">
</div>

### fork the following repo https://github.com/mahmoud254/Booster_CI_CD_Project and add dockerfile to run this django app and use github actions to build the docker image and push it to your dockerhub
#### Dockerfile
```
FROM ubuntu 
RUN apt update -y 
RUN apt-get install python3 -y
RUN apt install pip -y 
COPY . .
RUN pip3 install -r requirements.txt
RUN python3 manage.py makemigrations
RUN python3 manage.py migrate 
EXPOSE 8000
CMD ["python3","manage.py","runserver 0.0.0.0:8000"]
```
```
#### workflow
```
name: Docker Image CI

on:
  push:
    branches: [ "master" ]
  pull_request:
    branches: [ "master" ]

jobs:

  build:

    runs-on: ubuntu-latest

    steps:
    - uses: actions/checkout@v3
    - name: Build the Docker image
      run: |
           docker build . -f Dockerfile -t raniiaashraff/django-app:v1.0
           docker login -u ${{secrets.DOCKER_USERNAME}} -p ${{secrets.DOCKER_PASSWORD}} 
           docker push raniiaashraff/django-app:v1.0 

```

<div>
<img src="https://github.com/RaniiaAshraf/Jenkins/blob/main/day2/pics/19.png" width="800" height"500">

</div>
