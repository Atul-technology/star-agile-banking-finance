pipeline{
    agent any
     tools{
      maven 'M2_HOME'
           }
    stages{
      stage('Git checkout'){
          steps {
            echo 'this is for cloning git repo'
            git url:'https://github.com/ArunTvv/Banking-java-project.git'
          }
        }
     stage('Maven package'){
          steps {
            echo 'this is for packaging the application'
            sh 'mvn package'
          }
        }
     stage('Test Results'){
          steps {
            echo 'this is for generating test Results'
            publishHTML([allowMissing: false, alwaysLinkToLastBuild: false, icon: '', keepAll: false, reportDir: '/var/lib/jenkins/workspace/BankingProject/target/surefire-reports', reportFiles: 'index.html', reportName: 'HTML Report', reportTitles: '', useWrapperFileDirectly: true])
          }
        }
     stage('Docker image creation'){
          steps {
            echo 'this is for Docker image build'
            sh 'docker build -t aruntvv/bankingproject:latest .'
          }
        }
     stage('Login to DockerHub'){
          steps {
            echo 'this is for Login Dockerhub'
            withCredentials([usernamePassword(credentialsId: 'dockerid', passwordVariable: 'dockerpass', usernameVariable: 'dockeruser')]) {
            sh 'docker login -u ${dockeruser} -p ${dockerpass}'
            }
          }
        }
     stage('Push Docker image'){
          steps {
            echo 'this is for Pushing Docker image'
            sh 'docker push aruntvv/bankingproject:latest'
          }
        }
     stage('Deploy with ansible'){
          steps {
            echo 'this is for ansible'
           ansiblePlaybook credentialsId: 'sshkey', disableHostKeyChecking: true, installation: 'ansible', inventory: '/etc/ansible/hosts', playbook: 'deploy.yml', vaultTmpPath: ''
          }
        }
     }
   }

   
