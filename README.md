# CICD-Jenkins-Docker-ECS


# Prerequisites
 
- Install AWS cli and docker engine on the machine of Jenkins 
- Store AWS credentials in Jenkins
- Create ECR repo
- Create ECS cluster and service
- Install ECR, docker pipeline and AWS sdk for credentials plugins



# Technologies 
- Jenkins
- AWS ECR, ECS
- Sonarqube
- Maven


# Aim Of Project
In this CICD pipeline we are going to publish docker images we will use Jenkins to fetch the code after this use maven to run a unit test and we will use maven also to do analyse with checkstyle and again code analysis with sonarqube upload the result to sonarqube server waiting the quality gate checks after this we are going to build a docker image this docker image will contain the artifact after this we will publich the image to a registry called Amazon ECR now we have a dockerized application which we can deliver and host in ECS


# Flow OF Execution

- First we are going to install docker engine and aws cli on Jenkins server

- After that we have to Add Jenkins user to docker group ‘usermod -a -G docker jenkins’

- On AWS console create iam user create access key and secret access key and attach policy to this user to have container registry full access and ecs full 
  access permission and store these credentials in jenkins

- On Jenkins now time to install docker pipeline, amazon ECR, AWS sdk, cloudbees docker and pipeline AWS steps  plugins

- configure AWS credentials on Jenkins for the user we created in aws

- Now time to build our ECS cluster ‘AWS fargate’ infrastructure type, Task definition for running container, create service that will manage your container 
  ‘task’ and inside that service we will mention task definition that we created.
  
- Finally time to build our pipeline

 



