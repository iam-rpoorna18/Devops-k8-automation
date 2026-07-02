pipeline {
    agent any 
    // { label 'host' }
    stages{
        stage('Build Maven'){
            steps{
                checkout([$class: 'GitSCM', branches: [[name: '*/main']], extensions: [], userRemoteConfigs: [[url: 'https://github.com/iam-rpoorna18/Devops-k8-automation.git']]])
                sh 'mvn clean install'
            }
        }
        stage('Build docker image'){
            steps{
                script{
                    sh 'whoami'
                    sh 'docker build -t k8/devops-integration:latest .'
                    //sh 'docker tag k8/devops-integration:latest localhost:5000/k8/devops-integration:latest'
                    //sh 'docker push localhost:5000/k8/devops-integration:latest'
                }
            }
        }
        stage('Load Image into Kind') {
            steps {
                sh 'kind load docker-image k8/devops-integration:latest'
            }
        }
        stage('Deploy to k8s'){
            steps{
                script{
                   sh 'kubectl apply -f deploymentservice.yaml'
                }
            }
        }
    }
}
