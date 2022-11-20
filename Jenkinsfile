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
        stage ('SonarQube analysis') {
            steps {
                script {
                    withSonarQubeEnv(credentialsId: 'sonar-token') {
                      sh 'mvn clean package sonar:sonar'    
                    }
                }    
            }
        }  
        stage ('tomcat deployment') {
            steps {
                sshagent(['tomcat-server']) {
                    sh "scp -r target/Uber.jar ubuntu@172.31.36.176:/opt/tomcat/webapps"
                }
            }   
        }
    }      
}
