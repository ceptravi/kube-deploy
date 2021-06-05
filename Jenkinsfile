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
        
     stage('Deploy to k8s'){
            steps{
               
	
					sshagent(['id_ed25519']) {
    					 sh 'ssh -o StrictHostKeyChecking=no ravicept@cept.gov.in uptime'
            sh 'ssh -v user@hostname.com'
            sh 'scp ./source/filename ravicept@cept.gov.in:/remotehost/target'
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
        