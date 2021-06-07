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
        sshagent(credentials : ['id_ed25519']) {
           sh 'ssh -t ravi_cept@172.28.12.11 '
            sh 'ssh -v ravi_cept@172.28.12.11'
            sh 'scp ./source/filename ravi_cept@172.28.12.11:/remotehost/target'
        }
    }
}

     stage('Deploy to k8s'){
            steps{
               
	
					sshagent(['id_ed25519']) {
    					sh "scp -o StrictHostKeyChecking=no services.yml node-app-pod.yml ravi_cept@172.28.12.11:6443"
    					script{
    					    try{
    					        sh "ssh ravi_cept@172.28.12.11 kubectl apply -f ."
    					    	}
    					    catch(error){
    					        sh "ssh ravi_cept@172.28.12.11 kubectl create -f ."
    					    			}

    					  
    							}
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
        