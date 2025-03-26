pipeline {
    agent any
    tools {
        maven 'Maven'
    }
    stages {

        stage('Checkout the Code from GitHub') {
            steps {
                git url: 'https://github.com/Atul-technology/star-agile-banking-finance'
                echo 'GitHub repository checked out'
            }
        }

        stage('Code Compile with Atul') {
            steps {
                echo 'Starting code compilation'
                sh 'mvn compile'
            }
        }

        stage('Code Testing with Atul') {
            steps {
                echo 'Running unit tests'
                sh 'mvn test'
            }
        }

        stage('QA with Atul') {
            steps {
                echo 'Running code quality checks'
                sh 'mvn checkstyle:checkstyle'
            }
        }

        stage('Package with Atul') {
            steps {
                echo 'Packaging application'
                sh 'mvn clean package'
            }
        }

        stage('Build Docker Image') {
            steps {
                echo 'Building Docker image'
                sh 'docker build -t atul0074/bankingproject:latest .'
            }
        }

        stage('Docker Login Credentials') {
            steps {
                echo 'Logging into DockerHub'
                withCredentials([usernamePassword(credentialsId: 'dockeruserid', passwordVariable: 'dockerhubuserpw', usernameVariable: 'dockerhubusername')]) {
                    sh "docker login -u ${dockerhubusername} -p ${dockerhubuserpw}"
                }
            }
        }

        stage('Image Push to DockerHub') {
            steps {
                echo 'Pushing Docker image to DockerHub'
                sh 'docker push atul0074/bankingproject:latest'
            }
        }

        stage('Deploy Application using Ansible') {
            steps {
                echo 'Deploying application using Ansible playbook'
                ansiblePlaybook credentialsId: 'private-key', disableHostKeyChecking: true, installation: 'Ansible2', inventory: '/etc/ansible/hosts', playbook: 'ansible-playbook.yml', vaultTmpPath: ''
            }
        }
    }
}
