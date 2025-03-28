pipeline{
    agent any
      stages{
        stage('Git checkout'){
          steps{
                 git 'https://github.com/kavyaganji28/Banking-java-project'
                 echo 'This is for cloning the gitrepo'
            }
        }   
        stage('Create a Package'){
          steps{
                echo 'This will create a package using maven'
                sh 'mvn package'
            }
        }
        stage('Create a Docker image'){
          steps{
               sh 'docker build -t kavyaganji95/bankingfinance:latest .'

           }
         }
        stage('Login to Dockerhub'){
          steps{
              withCredentials([usernamePassword(credentialsId: 'dockercode', passwordVariable: 'dockerpasswd', usernameVariable: 'dockeruser')]) {
               sh 'docker login -u kavyaganji95 -p ${dockerpasswd}'
              }
           }
         }
        stage('Push the Docker image'){
            steps{
                   sh 'docker push kavyaganji95/bankingfinance:latest'
            }
        } 

      stage('Ansbile config and Deployment') {
         steps {
		ansiblePlaybook credentialsId: 'ssh', disableHostKeyChecking: true, installation: 'Ansible', inventory: '/etc/ansible/hosts', playbook: 'ansible-playbook.yml', vaultTmpPath: ''
            }
      }
          
      }
}	
