pipeline {
    agent any
    environment {
        registry = "204813321067.dkr.ecr.ap-south-1.amazonaws.com/dockerimage"
        region = "ap-south-1"
    }
    stages {
        stage('git checkout') {
            steps {
                checkout scmGit(branches: [[name: '*/main']], extensions: [], userRemoteConfigs: [[url: 'https://github.com/RAM28EC/springboot-app-DevOps-EKS.git']])
            }
        }
        stage ("build Jar") {
            steps {
                sh "mvn clean install"
            }
        }
        stage('Build Image') {
            steps {
                script{
                    docker.build registry
                }
            }
        }
        stage('push image') {
            steps {
                script{
                    sh "aws ecr get-login-password --region ap-south-1 | docker login --username AWS --password-stdin 204813321067.dkr.ecr.ap-south-1.amazonaws.com"
                    sh "docker push 204813321067.dkr.ecr.ap-south-1.amazonaws.com/dockerimage"

                }
            }
        }
 
        

    }
}