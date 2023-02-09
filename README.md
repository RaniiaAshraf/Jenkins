# Jenkins
## Day2
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
</div>
