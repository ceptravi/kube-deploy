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
    					sh "scp -o StrictHostKeyChecking=no services.yml node-app-pod.yml ravi_cept@172.28.12.11:/home/ravi_cept/"
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
        stage ('Deploy') {
    steps{
        sshagent(credentials : ['use-the-id-from-credential-generated-by-jenkins']) {
            sh 'ssh -o StrictHostKeyChecking=no user@hostname.com uptime'
            sh 'ssh -v user@hostname.com'
            sh 'scp ./source/filename user@hostname.com:/remotehost/target'
        }
    }
}
        stage('Apply Kubernetes Files') {
      steps {
          withKubeConfig([credentialsId: 'test-cluster']) {
          sh 'sudo -s'
         
          sh 'kubectl apply -f https://raw.githubusercontent.com/ceptravi/kube-deploy/main/deployment.yml'
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
        