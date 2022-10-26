pipeline {
    agent any
    stages {
        stage ('git checkout') {
            steps {
              git branch: 'main', url: 'https://github.com/sai3222/demo-counter-app-vikash.git'
            }
        }    
        stage ('unit testing') {
            steps {
                sh 'mvn test'
            }
        }    
        stage ('Integration Testing') {
            steps {
                sh 'mvn verify -DskipUnitTests'
            }
        }    
        stage ('Maven Build') {
            steps {
                sh 'mvn clean install'
            }    
        }
        stage ('SonarQube analysis') {
            steps {
                script {
                    withSonarQubeEnv(credentialsId: 'sonar-api-key') {
                      sh 'mvn clean package sonar:sonar'    
                }
                }    
            }
        }
   }     
}
