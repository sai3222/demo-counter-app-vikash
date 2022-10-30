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
      stage ('Upload jar nexus') {
          steps {
              script {
                def readPomVersion  = readMavenPom file: 'pom.xml'
                def nexusRepo = readMavenPom.Version.endswith("SNAPSHOT") ? "demo-snapshot" : "demo-app"
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
                nexusUrl: '65.1.129.41:8081', 
                nexusVersion: 'nexus3', 
                protocol: 'http', 
                repository: 'nexusRepo', 
                    version: "${readPomVersion.version}"
              }
          }
      }
      stage ('Docker image build') {
          steps {
              scripts {
                  sh 'docker image build -t $JOB_NAME:v1.$BUILD_ID .'
                  sh 'docker image tag $JOB_NAME:v1.$BUILD_ID saikrishna310/$JOB_NAME:v1.$BUILD_ID'
                  sh 'docker image tag $JOB_NAME:v1.$BUILD_ID saikrishna310/$JOB_NAME:latest'
              }
          }
      }
      stage ('Docker push'){
          steps {
              scripts {
               withCredentials([string(credentialsId: 'docker_crede', variable: 'docker_cred')]) {
               }
                  sh 'docker login -u saikrishna310 -p ${docker_cred} '
                sh 'docker image push saikrishna310/$JOB_NAME:v1.$BUILD_ID'
                sh 'docker image push saikrishna310/$JOB_NAME:latest'  
              }  
          }
      }  
    }      
}
