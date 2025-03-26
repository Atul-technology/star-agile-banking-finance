pipeline {
    agent any
    stages {
        stage('Git checkout') {
            steps {
                echo 'Cloning the Git repository'
                git url: 'https://github.com/Atul-technology/star-agile-banking-finance'
            }
        }
        stage('Maven package') {
            steps {
                echo 'Packaging the application using Maven'
                sh 'mvn clean package'
            }
        }
        stage('Test Results') {
            steps {
                echo 'Generating Test Results'
                publishHTML([allowMissing: false, alwaysLinkToLastBuild: false, reportDir: 'target/surefire-reports', 
                              reportFiles: 'index.html', reportName: 'Test Report'])
            }
        }
        stage('Docker image creation') {
            steps {
                echo 'Building Docker image'
                sh 'docker build -t atul0074/bankingproject:latest .'
            }
        }
        stage('Login to DockerHub') {
            steps {
                echo 'Logging in to DockerHub'
                withCredentials([usernamePassword(credentialsId: 'dockerid', passwordVariable: 'dockerpass', usernameVariable: 'dockeruser')]) {
                    sh 'echo ${dockerpass} | docker login -u ${dockeruser} --password-stdin'
                }
            }
        }
        stage('Push Docker image') {
            steps {
                echo 'Pushing Docker image to DockerHub'
                sh 'docker push atul0074/bankingproject:latest'
            }
        }
        stage('Deploy with Ansible') {
            steps {
                echo 'Deploying using Ansible'
                ansiblePlaybook credentialsId: 'sshkey', disableHostKeyChecking: true, installation: 'ansible', 
                inventory: '/etc/ansible/hosts', playbook: 'deploy.yml'
            }
        }
    }
}
