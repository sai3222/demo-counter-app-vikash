pipeline {
    agent any
    stages {
        stage ('git checkout') {
            steps {
              git branch: 'develop', url: 'https://github.com/sai3222/demo-counter-app-vikash.git'
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
        stage ('SonarQube-analysis') {
            steps {
                script {
                    withSonarQubeEnv(credentialsId: 'sonar_token') {
                      sh 'mvn clean package sonar:sonar'    
                    }
                }    
            }
        }
        
      stage ('Quality Gate status') {
            steps {
                script {
                    waitForQualityGate abortPipeline: false, credentialsId: 'sonar_token' {
                    }   
                }   
            }
      }   
    }      
}
