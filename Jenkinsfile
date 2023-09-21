pipeline {

    tools{

        maven 'maven3.9.4'
    }
    agent any

    environment {
        registry = "492552149929.dkr.ecr.ca-central-1.amazonaws.com/nextcloudapp"
    }
    stages {
        stage('Checkout') {
            steps {
                checkout scmGit(branches: [[name: '*/master']], extensions: [], userRemoteConfigs: [[url: 'https://github.com/Lion-Technology-Solutions/nextcloud-application-docker-pipeline.git']])
            }
        }
        
        stage ("Build Image") {
            steps {
                script {
                    dockerImage = docker.build registry
                    dockerImage.tag("$BUILD_NUMBER")
                }
            }
        }
        
        stage ("Push to ECR") {
            steps {
                script {
                    sh 'aws ecr get-login-password --region ca-central-1 | docker login --username AWS --password-stdin 492552149929.dkr.ecr.ca-central-1.amazonaws.com'
                    sh 'docker push 492552149929.dkr.ecr.ca-central-1.amazonaws.com/nextcloudapp:$BUILD_NUMBER'
                    
                }
            }
        }    
    }
}