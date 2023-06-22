pipeline {
    tools {
        maven 'Maven3'
    }
    agent any
    environment {
        registry = "204813321067.dkr.ecr.ap-south-1.amazonaws.com/my-docker-repo"
    }
    stages {
        stage('cloning git'){
            steps {
                checkout scmGit(branches: [[name: '*/main']], extensions: [], userRemoteConfigs: [[url: 'https://github.com/RAM28EC/springboot-app-DevOps-EKS.git']])
            }
        }
        stage('Build') {
            steps{
                sh 'mvn clean install'
            }
        }
        stage('Build Docker image') {
            steps{
                script{
                    dockerImage = docker.build registry
                }
            }
        }
        stage('push imgae to ecr'){
            steps{
                script{
                    sh 'aws ecr get-login-password --region ap-south-1 | docker login --username AWS --password-stdin 204813321067.dkr.ecr.ap-south-1.amazonaws.com'
                    sh 'docker push 204813321067.dkr.ecr.ap-south-1.amazonaws.com/my-docker-repo:latest'
                }
            }
        }
        stage('k8s deploy'){
            steps{
                script{
                    withKubeConfig(caCertificate: '', clusterName: '', contextName: '', credentialsId: 'K8S', namespace: '', restrictKubeConfigAccess: false, serverUrl: '') {
                    sh ('kubectl apply -f eks-deploy-k8s.yaml')    
                    }
                }
            
            }
        }
    }
    
}
