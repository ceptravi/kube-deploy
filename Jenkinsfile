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
        
        stage('Deploying Docker Image') {
			steps {
				script {
					 kubernetesDeploy(
                                credentialsType: 'KubeConfig',
                                kubeConfig: [path: '/var/lib/jenkins/workspace/.kube/config'],
                                configs: 'mypods-deployment.yml', 
                                dockerCredentials: [
                                      [credentialsId: 'my_registry_creds_id'],
                                ],
					)
				}
			}
		}
        
         stage('Deploy App') {
      steps {
        script {
          kubernetesDeploy(configs: "myweb.yaml", kubeconfigId: "mykubeconfig")
        }
      }
    }
       
       


}
}
