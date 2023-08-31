pipeline {
    agent any
    tools{
        maven 'maven_3_5_0'
    }
    stages{
        stage('Build Maven') {
            steps{
              checkout scmGit(branches: [[name: '*/main']], extensions: [], userRemoteConfigs: [[url: 'https://github.com/tahabozdemir/SimpleJavaProject']])
                sh 'mvn clean install'
            }
        }
        stage('Build docker image') {
            steps{
                script{
                    sh 'docker build -t 63.35.99.174:5000/devops-integration .'
                }
            }
        }
        stage('Push image to S3 Bucket') {
            steps{
                script{ 
                   sh 'docker push 63.35.99.174:5000/devops-integration'
                }
            }
        }
    }
}