pipeline {
    agent any
  
    
    stages{
        stage('Build Docker Image'){
            steps{
                
                sh "docker build . -t ceptravi/kube-deploy:latest"
            }
        }
        stage('Docker Push'){
            steps{
              withCredentials([usernamePassword(credentialsId: 'dockerhub', passwordVariable: 'pass', usernameVariable: 'user')]) {
                    
       					sh "docker login --username=ceptravi --password=anitscse51"
       					sh "docker push ceptravi/kube-deploy:latest"
				}
            }
        }
        
        stage ('Deploy') {
    steps{
        sshagent(credentials : ['1SbQN0zh4GcMgOqiX7sRdWBkbwfJTL5E4']) {
       
           sh 'ssh -t -t ravi_cept@172.28.12.11'
           sh 'sudo -s'
           sh 'kubectl get pods'
        }
    }
}

        
        
    	
}
post {
    success {
      slackSend(message: "Pipeline is successfully completed.")
    }
    failure {
      slackSend(message: "Pipeline failed. Please check the logs.")
    }
}
}      
        