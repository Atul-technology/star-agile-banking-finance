pipeline{
    agent any
    tools {
           maven 'Maven' // Change 'Maven3' to 'Maven' if it's installed as Maven
         }
    stages{
        stage('checkout the code from github'){
            steps{
                 git 'https://github.com/Atul-technology/star-agile-banking-finance'
                 echo 'github url checkout'
            }
        }
        stage('code compile with atul'){
            steps{
                echo 'starting compiling'
                sh 'mvn compile'
            }
        }
        stage('code testing with atul'){
            steps{
                sh 'mvn test'
            }
        }
        stage('qa with atul'){
            steps{
                sh 'mvn checkstyle:checkstyle'
            }
        }
        stage('package with atul'){
            steps{
                sh 'mvn clean package'
            }
        }
        stage('Build Docker image'){
          steps{
               sh 'docker build -t atul0074/bankingproject:latest .'

           }
         }
        stage('Docker Login Credenials'){
          steps{
              withCredentials([usernamePassword(credentialsId: 'dockeruserid', passwordVariable: 'dockerhubuserpw', usernameVariable: 'dockerhubusername')]) {
               sh "docker login -u ${dockerhubusername} -p ${dockerhubuserpw}"
              }
           }
         }
        stage('Image push to Dockerhub'){
            steps{
                   sh 'docker push atul0074/bankingproject:latest'
            }
        } 

      stage('Deploy Application using Ansible') {
         steps {
		ansiblePlaybook credentialsId: 'private-key', disableHostKeyChecking: true, installation: 'Ansible2', inventory: '/etc/ansible/hosts', playbook: 'ansible-playbook.yml', vaultTmpPath: ''
            }
      }
          
      }
}	
