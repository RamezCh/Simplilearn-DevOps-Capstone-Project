# Project 2

## Description

Create an automated test environment for continuous evaluation of the application.

## Background of the problem statement:

An entertainment company like BookMyShow where users book their tickets have multiple users accessing their web app. Due to less infrastructure availability, they use less machines and provide the required structure. This method includes many weaknesses such as:

- Developers must wait till the complete software development for the test results.
- There is a huge possibility of bugs in the test results.
- Delivery process of the software is slow.
- The quality of software is a concern due to continuous feedback referring to things like coding or architectural issues, build failures, test conditions, and file release uploads.
- The objective is to implement the automation of the build and release process for
their product.

## Implementation requirements:

Set up the Jenkins server in master or slave architecture
Use the Jenkins plugins to perform the computation part on the Docker containers
Create Jenkins pipeline script
Use the GIT web hook to schedule the job on check-in or poll SCM
Build an image using the artifacts and deploy them on containers
Remove the container stack after completing the job

## The following tools must be used:

- EC2
- Jenkins
- Docker
- Ansible, Chef, or Puppet

## The following things to be kept in check:

You need to document the steps and write the algorithms in them.

The submission of your GitHub repository link is mandatory. In order to track your tasks, you need to share the link of the repository.

Document the step-by-step process starting from creating test cases, then executing it, and recording the results

## You need to submit the final specification document, which includes:

- Project and tester details
- Concepts used in the project
- Links to the GitHub repository to verify the project completion
- Your conclusion on enhancing the application and defining the USPs (Unique Selling Points)


## Solution

### Step 1: Set up of Server for project execution

- Connect to AWS LAB
- Create EC2 server on AWS
- Install git, jenkins & docker
- Connect to Jenkins Dashboard
- Configure Maven

### Step 2: Create a Build pipeline

Write pipeline code:

- clone repo
- test code
- build code

### Step 3: Deploy Code

- Build Dockerfile
- Run Img to create container & deploy application

### Step 4:

- Set up webhook trigger on jenkins for continous integration and deployment


## Applying Solution:

### Step 1:

AWS Lab -> EC2 -> Launch Instance -> Change Region to US-East (Virginia) -> Name it CapstoneProject -> Ubuntu -> 22.04 -> Create new key pair -> 02Sep key name -> Edit near network -> add security group rule -> All traffic instead of custom TCP -> Anywhere -> Launch instance -> go to instances -> select CapstoneProject instance and connect -> type below commands

sudo su -

git --version

#### Install java

apt-get update

sudo apt install default-jre -y

java -version

#### Download Jenkins

sudo wget -O /usr/share/keyrings/jenkins-keyring.asc https://pkg.jenkins.io/debian-stable/jenkins.io-2023.key

echo "deb [signed-by=/usr/share/keyrings/jenkins-keyring.asc]" https://pkg.jenkins.io/debian-stable binary/ | sudo tee /etc/apt/sources.list.d/jenkins.list > /dev/null

apt-get update

apt-get install jenkins -y

systemctl start jenkins


systemctl status jenkins

Take the screenshot for above command output

press q to come out of above command

#### Install Docker:

sudo apt update

sudo apt install apt-transport-https ca-certificates curl software-properties-common -y

curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -
sudo add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu focal stable"

apt-cache policy docker-ce

sudo apt install docker-ce -y

sudo systemctl status docker

Take the screenshot for above command output

press q to come out of above command


#### Connect to Jenkins Dashboard

take public IP at bottom from EC2 Instance we created, go to browser and give ip:8080 then go to EC2 instance again and cat /var/lib/jenkins/secrets/initialAdminPassword and it will show pass then copy and paste it then login -> install suggested plugins (if error appears reboot instance) -> create first admin account (all admin) -> save and continue -> save and finish -> start using Jenkins


#### Configure Maven

Manage Jenkins -> Tools -> scroll down -> Add Maven -> name it mymaven then save -> new item -> CapstoneProject2-CICDpipline -> create pipline

pipeline {
  agent any
  tools{
  maven 'mymaven'
  }
  stages{
    stage('Checkout Code'){
      steps{git 'https://github.com/Sonal0409/MavenBuild-SL.git'}
    }
    stage('Build the Code'){
      steps{
      sh 'mvn package'
      }
    }
    stage('Build the Image'){
      steps{
      sh 'docker build -t capstoneproject:$BUILD_NUMBER .'
      }
    }
  }
}

fork https://github.com/Sonal0409/MavenBuild-SL then go to settings -> webhook -> add webhook -> http://54.242.125.121:8080/github-webhook/ or your instance public ip:8080/github-webhook
