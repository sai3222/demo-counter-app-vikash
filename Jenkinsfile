pipeline {
    agent any
    stages{
        stage ('git checkout'){
            steps {
              git
            }
        stage ('unit testing'){
            steps {
                sh 'mvn test'
            }    
        stage ('Integration Testing'){
            steps {
                sh 'mvn verify -DskipUnitTests'
            }    
        stage ('Maven Build'){
            steps {
                sh 'mvn clean install'
            }    
        }
    }
}
