![Blank diagram (2)](https://github.com/user-attachments/assets/f0817ee6-612d-4397-b5f1-55739869079a)

Prerequisite tools for creating the Jenkins project 
•	Linux machine 
•	Maven 
•	Git Hub 
•	Jenkins 
•	Docker
•	JDK
•	NPM
•	Nodejs

Step-1: Jenkins Server Setup in Linux VM

Create Ubuntu VM using AWS EC2 (t2.medium)

Enable 8080 Port Number in Security Group Inbound Rules TCP

Connect to VM using git bash

Install Java Good practice Update the machine -> sudo apt-get update Install the java now -> sudo apt-get install openjdk-17-jre or sudo yum install openjdk-17-jre

Install Jenkins * Jenkins to be installed

sudo wget -O /usr/share/keyrings/jenkins-keyring.asc \ https://pkg.jenkins.io/debian-stable/jenkins.io-2023.key echo deb [signed-by=/usr/share/keyrings/jenkins-keyring.asc] \ https://pkg.jenkins.io/debian-stable binary/ | sudo tee \ /etc/apt/sources.list.d/jenkins.list > /dev/null

       sudo apt-get update
       sudo apt-get install jenkins 

    * start Jenkins
         sudo systemctl enable jenkins
         sudo systemctl start jenkins

     * verify Jenkins
          sudo systemctl status jenkins

     * access Jenkins server in browser using VM public ip 
         http://public-ip:8080/
         http://15.206.209.14:8080/

     * copy Jenkins admin pwd, Create Admin Account & Install Required Plugins in Jenkins
          **sudo more /var/lib/jenkins/secrets/initialAdminPassword** or **sudo cat /var/lib/jenkins/secrets/initialAdminPassword**
          password: 618d55d61e0840e2b113cd953d6916eb
Create Admin Account & Install Required Plugins in Jenkins **Note: I have created this job using the pipeline option. not the freestyle. The freestyle-only beginner will also use very simple project. we can go with freestyle

** There are Two ways to create or write the pipeline. 1, Declarative approach 2, Scripted approach****

Interview notes: To identify the script -> starting with pipeline is called the Declarative approach and the script starting with None is called the Scripted approach. ** I am doing this job with a Declarative approach **

again because this is very important Interview notes: To identify the script -> starting with pipeline is called the Declarative approach and the script starting with None is called the Scripted approach. ** I am doing this job with a Declarative approach **


pipeline {
    agent any
    
    stages {
        stage('Git SCM') {
            steps {
                git 'https://github.com/Santhunew/project-2-jenkins-nodejs-docker.git'
            }
        }
        stage('Run Test') {
            steps {
                sh 'npm test'
                sh 'npm --version'
                sh 'node --version'
            }
        }
        stage('Build') {
            steps {
                sh 'npm run build'
            }
        }
        stage('Build Docker Image') {
            steps {
                script{
                sh 'docker build -t santhoshss7878/nodejs-app:1.0 .'
                }
            }
        }
        stage('Docker Image push') {
            steps {
                script{
                withCredentials([string(credentialsId: 'dockerhub-pwd', variable: 'dockerhubpwd')]) {
                sh 'docker login -u santhoshss7878 -p ${dockerhubpwd}'
                sh 'docker push santhoshss7878/nodejs-app:1.0'
                   }
                }
            }
        }
    }
}


Notes to add variable in jenkins: this will help us to establish the password from jenkins to docker.
![image](https://github.com/user-attachments/assets/c6541336-d972-4bc3-bf74-9b478b25b927)

![image](https://github.com/user-attachments/assets/b6069b4a-5e24-41b9-821e-35e47d04bb5b)

![image](https://github.com/user-attachments/assets/82fe2f2b-02ea-4891-8e04-a8e7ce5c68f7)




