pipeline {
    agent any
 
    tools {
        maven 'Apache Maven 3.8.7' // Ensure this matches Jenkins Maven configuration
    }
 
    environment {
        DOCKER_IMAGE = 'rojabonakurthi96/bankingproject:latest'
    }
 
    stages {
        stage('Checkout Code') {
            steps {
                echo 'Cloning repository from GitHub...'
                git url: 'https://github.com/rojabonakurthi96/star-agile-banking-finance.git'
            }
        }
 
        stage('Compile Code') {
            steps {
                echo 'Compiling code...'
                sh 'mvn compile'
            }
        }
 
        stage('Run Tests') {
            steps {
                echo 'Running unit tests...'
                sh 'mvn test'
            }
        }
 
        stage('Code Quality Check') {
            steps {
                echo 'Running code quality checks...'
                sh 'mvn checkstyle:checkstyle'
            }
        }
 
        stage('Package Application') {
            steps {
                echo 'Packaging application...'
                sh 'mvn clean package'
            }
        }
 
        stage('Build Docker Image') {
            steps {
                echo 'Building Docker image...'
                sh "docker build -t $DOCKER_IMAGE ."
            }
        }
 
        stage('Docker Login') {
            steps {
                echo 'Logging into DockerHub...'
                withCredentials([
                    usernamePassword(credentialsId: 'dockerhubusername', usernameVariable: 'DOCKER_USER', passwordVariable: 'DOCKER_PASS')
                ]) {
                    sh 'echo $DOCKER_PASS | docker login -u $DOCKER_USER --password-stdin'
                }
            }
        }
 
        stage('Push Image to DockerHub') {
            steps {
                echo 'Pushing Docker image to DockerHub...'
                sh "docker push $DOCKER_IMAGE"
            }
        }
 
        stage('Deploy Using Ansible') {
            steps {
                echo 'Deploying application using Ansible playbook...'
                ansiblePlaybook become: true, 
                                credentialsId: 'ansible', 
                                disableHostKeyChecking: true, 
                                installation: 'ansible', 
                                inventory: '/etc/ansible/hosts', 
                                playbook: 'ansible-playbook.yml'
            }
        }
    }
}
