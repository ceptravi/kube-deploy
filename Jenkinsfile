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
        
       
             	stage('Apply Kubernetes files') {
             	steps{
    				withKubeConfig([credentialsId: 'kubeconfig', serverUrl: 'https://172.28.12.11:6443']) {
    				sh 'sudo -s'
      				sh 'kubectl apply -f my-kubernetes-directory'
      				}
    }
  }
           



}
}
