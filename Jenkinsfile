pipeline {
    agent any    
    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', credentialsId: 'GitHub_creds', url: 'https://github.com/jayashri27/k8s_demo'
            }
        }
        
        stage ("build Jar") {
            steps {
                sh "chmod +x gradlew"
                sh "./gradlew build"
            }
        }
        
        stage ("Docker build") {
            steps{  
                {
         sh 'docker version'
         sh 'docker build -t kubernetes-demo .'
         sh 'docker image list'
         sh 'docker tag kubernetes-demo jayashrimahale22/kubernetes-demo:kubernetes-demo'
          }
        }
        }
        stage ("dokerlogin"){
            steps{
                 withCredentials([string(credentialsId: 'Docker_Hub_Passwd', variable: 'PASSWORD')]) {
                 sh 'docker login -u jayashrimahale22 -p $PASSWORD'
        }
            }
        }
        
        stage ("Push image to docker hub") {
            steps {
                 sh 'docker push  jayashrimahale22/kubernetes-demo:kubernetes-demo'
            }
        }
        
        stage ("Deploy to K8S") {
            steps {
                withKubeConfig(caCertificate: '', clusterName: '', contextName: '', credentialsId: 'K8S', namespace: '', restrictKubeConfigAccess: false, serverUrl: '') {
                sh "kubectl apply -f eks-deploy-k8s.yaml"
                    
                }
            }
        }
    }
}
