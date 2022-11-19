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
                sh 'mvn clean package'
            }
        }
        stage ('tomcat server') {
            steps {
                sshagent(['tomcat-server']) {
                }
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
    }      
}
