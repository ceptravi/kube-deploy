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
       
          stage('Deploy to dev'){
            steps{
            kubernetesDeploy(kubeconfigId: 'ravi_cept@172.28.12.11:/home/ravi_cept/',               // REQUIRED

                 configs: '~/.kube/config', // REQUIRED
                 enableConfigSubstitution: false,
        
                 secretNamespace: 'default',
                 secretName: 'dashboard-token-chnwr',
                 dockerCredentials: [
                        [credentialsId: 'ceptravi'],
                        [credentialsId: 'ceptravi', url: 'https://index.docker.io/v1'],
                 ]
)
            }
        }
        
        stage('Deploy K8 practise'){
            steps{
            kubernetesDeploykubernetesDeploy configs: 'apiVersion: v1 clusters: - cluster:     certificate-authority-data: DATA+OMITTED     server: https://172.28.12.11::/home/ravi_cept/   name: kubernetes contexts: - context:     cluster: kubernetes     user: kubernetes-admin   name: kubernetes-admin@kubernetes current-context: kubernetes-admin@kubernetes kind: Config preferences: {} users: - name: kubernetes-admin   user:     client-certificate-data: REDACTED     client-key-data: REDACTED', kubeConfig: [path: ''], kubeconfigId: 'mykubeconfig', secretName: 'default-token-rkpxh', ssh: [sshCredentialsId: '*', sshServer: ''], textCredentials: [certificateAuthorityData: '', clientCertificateData: '', clientKeyData: '', serverUrl: 'https://']
            }
        }
        stage('Deploy to k8s'){
            steps{
               
	
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
