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
       
          sh 'sshpass -p "Mysuru@123" ssh -o StrictHostKeyChecking=no ravi_cept@172.28.12.11'
          sh 'sshpass -p "Mysuru@123" ssh -v ravi_cept@172.28.12.11'
          sh 'sshpass -p "Mysuru@123"  ssh ravi_cept@172.28.12.11 "kubectl --kubeconfig=/home/ravi_cept@cept.gov.in/Desktop/kubernetFolder/config apply -f https://raw.githubusercontent.com/ceptravi/kube-deploy/main/deployment.yml"'
          sh 'sshpass -p "Mysuru@123"  ssh ravi_cept@172.28.12.11 "kubectl --kubeconfig=/home/ravi_cept@cept.gov.in/Desktop/kubernetFolder/config get deployments"'
         
          
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
        