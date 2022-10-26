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
        stage ('SonarQube-analysis') {
            steps {
                script {
                    withSonarQubeEnv(credentialsId: 'sonar-api-key') {
                      sh 'mvn clean package sonar:sonar'    
                }
                }    
            }
        }
      stage ('Quality Gate status') {
            steps {
                script {
                  waitForQualityGate abortPipeline: false, credentialsId: 'sonar-api-key'
                }    
            }
      }
      stage ('Upload War to nexus') {
          steps {
              script {
                nexusArtifactUploader artifacts: 
                [
                     [
                            artifactId: 'springboot', 
                            classifier: '', file: 'target/Uber.jar',
                            type: 'jar'
                            ]
                ], 
                credentialsId: 'nexus-auth', 
                groupId: 'com.example', 
                nexusUrl: 'http://3.18.107.201:8081', 
                nexusVersion: 'nexus3', 
                protocol: 'http', 
                repository: 'Demoapp-release', 
                version: '1.0.0'  
              }
          }
      }    
   }     
}
