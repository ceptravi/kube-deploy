pipeline {
     
    
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
        
       
       
       stage('List pods') {
       steps{
    withKubeConfig([credentialsId: 'kubeconfig',
                    caCertificate: '<ca-certificate>',
                    serverUrl: 'https://172.28.12.11:6443',
                    contextName: 'kubernetes',
                    clusterName: 'kubernetes-admin@kubernetes',
                    namespace: 'default'
                    ]) {
      sh 'kubectl get pods'
    }
    }
  }
             	stage('Apply Kubernetes files') {
             	steps{
    				kubernetesDeploy (configs: 'apiVersion: v1 clusters: - cluster:     certificate-authority-data: DATA+OMITTED     server: https://172.28.12.11:6443   name: kubernetes contexts: - context:     cluster: kubernetes     user: kubernetes-admin   name: kubernetes-admin@kubernetes current-context: kubernetes-admin@kubernetes kind: Config preferences: {} users: - name: kubernetes-admin   user:     client-certificate-data: REDACTED     client-key-data: REDACTED', kubeConfig: [path: ''], kubeconfigId: 'mykubeconfig', secretName: 'default-token-rkpxh', ssh: [sshCredentialsId: '*', sshServer: ''], textCredentials: [certificateAuthorityData: '', clientCertificateData: '', clientKeyData: '', serverUrl: 'https://'])
    }
  }
           



}
agent {
      kubernetes {
      
        yamlFile 'pods.yml'
       
      }
    }
}
