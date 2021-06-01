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
                withCredentials([string(credentialsId: 'docker-hub', variable: 'dockerhubPwd')]) {
                    sh "docker login -u ceptravi -p ${dockerPwd}"
                    sh "docker push ceptravi/kude-deploy:latest"
                }
            }
        }
        stage('Deploy to k8s'){
            steps{
               
					sh "chmod +x changeTag.sh"
					sh "./changeTag.sh ${DOCKER_TAG}"
					sshagent(['kube-machine']) {
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
       
       
    }



}
