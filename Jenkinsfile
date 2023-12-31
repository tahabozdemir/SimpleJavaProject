def commit_id
node {
checkout scm
sh 'git rev-parse --short HEAD > .git/commit-id'
commit_id = readFile('.git/commit-id').trim()
}
pipeline {
    agent any
    tools {
        jdk 'jdk11'
        maven 'maven_3_5_0'
    }
    stages {
        stage('Build Maven') {
            steps {
              checkout scmGit(branches: [[name: '*/main']], extensions: [], userRemoteConfigs: [[url: 'https://github.com/tahabozdemir/SimpleJavaProject']])
              sh 'mvn clean install'
            }
        }
        
        stage('Build and push docker image') {
            steps {
                script {
                    docker.build("34.250.76.116:5000/tahabzd:${commit_id}", '.').push()
                }
            }
        }

        stage('Docker remove image from EC2') {
            steps {
                script {
                    sh "docker image rm -f 34.250.76.116:5000/tahabzd:${commit_id}"
                }
            }
        }

         stage('SonarQube analysis') {
             steps {
             withSonarQubeEnv('sonar-server') { 
                sh 'mvn sonar:sonar'
             }
          }
        }
    }
}
