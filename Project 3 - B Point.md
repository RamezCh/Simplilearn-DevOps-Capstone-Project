# B-Safe

# Course-end Project 3


## Description

Create a CI/CD Pipeline to convert the legacy development process to a DevOps process.

## Background of the problem statement:

A leading US healthcare company, Aetna, with a large IT structure had a 12-week release cycle and their business was impacted due to the legacy process. To gain
true business value through faster feature releases, better service quality, and cost optimization, they wanted to adopt agility in their build and release process.
The objective is to implement iterative deployments, continuous innovation, and automated testing through the assistance of the strategy.

## Implementation requirements:

- Install and configure the Jenkins architecture on AWS instance
- Use the required plugins to run the build creation on a containerized platform
- Create and run the Docker image which will have the application artifacts
- Execute the automated tests on the created build
- Create your private repository and push the Docker image into the repository
- Expose the application on the respective ports so that the user can access the deployed application
- Remove container stack after completing the job

## The following tools must be used:

- EC2
- Jenkins
- Docker
- Git

## The following things to be kept in check:

- You need to document the steps and write the algorithms in them.
- The submission of your Github repository link is mandatory. In order to track your tasks, you need to share the link of the repository.
- Document the step-by-step process starting from creating test cases, the executing it, and recording the results.

## You need to submit the final specification document, which includes:

- Project and tester details
- Concepts used in the project
- Links to the GitHub repository to verify the project completion
- Your conclusion on enhancing the application and defining the USPs (Unique Selling Points)

## Solution: 

Similar to Hangout Project but in Jenkins we need to set Docker Credentials and change pipeline script to match this use case.

sudo su -

git --version

### Install java

apt-get update

sudo apt install default-jre -y

java -version

### Download Jenkins

sudo wget -O /usr/share/keyrings/jenkins-keyring.asc https://pkg.jenkins.io/debian-stable/jenkins.io-2023.key

echo "deb [signed-by=/usr/share/keyrings/jenkins-keyring.asc]" https://pkg.jenkins.io/debian-stable binary/ | sudo tee /etc/apt/sources.list.d/jenkins.list > /dev/null

apt-get update

apt-get install jenkins -y

systemctl start jenkins


systemctl status jenkins


Take the screenshot for above command output

press q to come out of above command


### Install Docker:

sudo apt update

sudo apt install apt-transport-https ca-certificates curl software-properties-common -y

curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -
sudo add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu focal stable"

apt-cache policy docker-ce

sudo apt install docker-ce -y

sudo systemctl status docker

Take the screenshot for above command output

press q to come out of above command


### Setup jenkins dashboard

Copy the Ec2 server Public IP address
Open a new browser

copy in the URL :  http://publicIP:8080

Now go to the terminal and execute below command

cat /var/lib/jenkins/secrets/initialAdminPassword

Copy the output password

and paste it on the jenkins webpage

Click on Continue

Click on install suggested plugins

Next page

enter username: admin

passoerd as admin

confirm passwd: admin

full name as: admin

email: admin@gmail.com


Click on continue

Click on save and finish

Take screenshot

You will be on jenkins dashboard

Click on Manage Jenkins --> Click on tools --> Scroll down --> click on Add Maven --> Give name as mymaven --> save the changes.

Take screenshot

Now go to terminal of ec2 instance and run below command

chmod 777 /var/run/docker.sock





Create a new Job --> select project as pipeline -->press Ok --> in Pipeline tab give below code

Before executing the pipleine give jenkins permsision to run docker commands

 chmod 777 /var/run/docker.sock

Create the following pipeline in jenkins

pipeline{
    tools{
        maven 'mymaven'
    }
    agent any
    stages{
        stage('clone repo'){
            steps{
                git 'https://github.com/Sonal0409/DevOpsCodeDemo.git'
            }
        }
        stage('Build Code'){
            steps{
                sh 'mvn package'
            }
        }
        
        
        stage('build Image'){
          
            steps{
                sh 'cp /var/lib/jenkins/workspace/CICDpipeline/target/addressbook.war .'
                sh 'docker build -t myaddressbook .'
            }
        }
        
        stage('push Image'){
            steps{

withCredentials([string(credentialsId: 'DOCKER_HUB_PASWD', variable: 'DOCKER_HUB_PASWD')]) {
                sh 'docker login -u edu123 -p ${DOCKER_HUB_PASWD}'
                }
              
                sh 'docker tag myaddressbook edu123/myaddressbook'
                sh 'docker push edu123/myaddressbook'
            }
        }
        
        stage('Deploy container'){
            steps{
                sh 'docker run -d -P edu123/myaddressbook'
            }
        }
    }
}

Save the job and run it

