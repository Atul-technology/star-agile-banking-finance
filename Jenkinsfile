pipeline {
    agent any
    environment {
        IMAGE_NAME = 'myimg'
        CONTAINER_NAME = 'c01'
        HOST_PORT = '8092'
        CONTAINER_PORT = '8091'
    }
    stages {
        stage('Checkout Code') {
            steps {
                echo 'Checking out code from GitHub...'
                git url: 'https://github.com/Atul-technology/star-agile-banking-finance/'
            }
        }

        stage('Compile Code') {
            steps {
                echo 'Starting code compilation with Maven...'
                sh 'mvn compile'
            }
        }

        stage('Run Tests') {
            steps {
                echo 'Running tests with Maven...'
                sh 'mvn test'
            }
        }

        stage('Code Quality Check') {
            steps {
                echo 'Running Checkstyle for code quality...'
                sh 'mvn checkstyle:checkstyle'
            }
        }

        stage('Package Application') {
            steps {
                echo 'Packaging application with Maven...'
                sh 'mvn package'
            }
        }

        stage('Build Docker Image') {
            steps {
                echo 'Building Docker image...'
                sh "docker build -t ${IMAGE_NAME} ."
            }
        }

        stage('Stop Existing Container') {
            steps {
                echo 'Stopping any existing container if running...'
                script {
                    sh "docker stop ${CONTAINER_NAME} || true"
                    sh "docker rm ${CONTAINER_NAME} || true"
                }
            }
        }

        stage('Run Docker Container') {
            steps {
                echo 'Running Docker container...'
                sh "docker run -dt -p ${HOST_PORT}:${CONTAINER_PORT} --name ${CONTAINER_NAME} ${IMAGE_NAME}"
            }
        }

        stage('Deploy using Ansible') {
            steps {
                echo 'Deploying application using Ansible...'
                ansiblePlaybook become: true, credentialsId: 'ansible', disableHostKeyChecking: true, installation: 'ansible', inventory: '/etc/ansible/hosts', playbook: 'ansible-playbook.yml', sudoUser: null, vaultTmpPath: ''
            }
        }
    }
}

