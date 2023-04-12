pipeline {
    agent any
    
    environment {
		DH_CREDENTIALS=credentials('dh-cred')
	}
    
    stages{
        stage('Checkout'){
            steps{
                checkout([$class: 'GitSCM', branches: [[name: '*/main']], extensions: [], userRemoteConfigs: [[url: 'https://github.com/brittosj/k8s-testing']]])
                }
        }

        stage('Build docker image'){
            steps{
                    sh 'docker build -t brittosj/httpd:3.0 .'
		    sh 'echo $DH_CREDENTIALS_PSW | docker login -u $DH_CREDENTIALS_USR --password-stdin'
		    sh 'docker push brittosj/httpd:3.0'
		}
             }
        
         stage('Deploy to k8s'){
            steps{
                script{
                    kubernetesDeploy configs: 'pod-service.yml', kubeconfigId: 'k8s'
                }
            }
        }
    }
}
