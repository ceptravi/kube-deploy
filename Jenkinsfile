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
        stage('Deploy to kube step1'){
        steps{
             withCredentials([usernamePassword(credentialsId: 'kubernetes', passwordVariable: 'Mysuru@123', usernameVariable: 'user')]) {
                    
       					sh "kubectl apply -f ."
       					sh "docker push ceptravi/kube-deploy:latest"
       					}
       					}
				}
       
          stage('Deploy to dev'){
            steps{
            kubernetesDeploy configs: 'apiVersion: v1 clusters: - cluster:     certificate-authority-data: DATA+OMITTED     server: https://172.28.12.11:6443   name: kubernetes contexts: - context:     cluster: kubernetes     user: kubernetes-admin   name: kubernetes-admin@kubernetes current-context: kubernetes-admin@kubernetes kind: Config preferences: {} users: - name: kubernetes-admin   user:     client-certificate-data: REDACTED     client-key-data: REDACTED', kubeConfig: [path: '~/.kube/config'], kubeconfigId: 'mykubeconfig', secretName: 'jenkins-token-5b2pv', ssh: [sshCredentialsId: 'kube-machine', sshServer: 'https://172.28.12.11:6443'], textCredentials: [certificateAuthorityData: '', clientCertificateData: '', clientKeyData: 'Data ==== ca.crt:     1025 bytes namespace:  7 bytes token:      eyJhbGciOiJSUzI1NiIsImtpZCI6IkFQTFE0cGE5bWIxSU9QYXM4Yy1oM3k0Q2hYQ1ZvWWVTZnBkTVNRXzlwb2cifQ.eyJpc3MiOiJrdWJlcm5ldGVzL3NlcnZpY2VhY2NvdW50Iiwia3ViZXJuZXRlcy5pby9zZXJ2aWNlYWNjb3VudC9uYW1lc3BhY2UiOiJkZWZhdWx0Iiwia3ViZXJuZXRlcy5pby9zZXJ2aWNlYWNjb3VudC9zZWNyZXQubmFtZSI6ImplbmtpbnMtdG9rZW4tNWIycHYiLCJrdWJlcm5ldGVzLmlvL3NlcnZpY2VhY2NvdW50L3NlcnZpY2UtYWNjb3VudC5uYW1lIjoiamVua2lucyIsImt1YmVybmV0ZXMuaW8vc2VydmljZWFjY291bnQvc2VydmljZS1hY2NvdW50LnVpZCI6ImY3ZWJkOGFhLTdlNjItNGM5MC04Nzg2LWVkMDZmZDEzMWRiMCIsInN1YiI6InN5c3RlbTpzZXJ2aWNlYWNjb3VudDpkZWZhdWx0OmplbmtpbnMifQ.A8jcBSbShtc9u9v7X8SkNTNNXRpo_8yyfGtZTDM9yTKWBEarqPygq0gROizJuVcSljBJ_x2kbTpRpoMyWmQznM84mqlbHwEam2P4Jg1boGWo2SbeD5hQ7aQMu0dOBqMsLasiHMcs0YKV0tTfV_nw-xFiqQddRT6Pzg_JdOA3Yj87W4_Nt7NDJlWwb0WKlVXnHm-AEnJ2e_bjj4VN_oRZpLZF0sagg-brV59rRxxZ-AMqdBxdegyk_eQbdkAvwMksvwPhVJGuR96SyMqsrDL7GesQeAKdB_UKYLIFeESVpJf0NNEgy0WiEXWdLgtI3GKmL0AFTYyuHL5uwouRvPbSxg', serverUrl: 'https://172.28.12.11:6443']
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
