pipeline {
    agent any
    tools {
        maven 'maven'
    }
    stages {
        stage('Build') {
            steps {
                bat 'mvn clean install -DskipTests'
            }
        }
        stage('Testing'){
            steps{
                //sonarqube
                bat 'mvn test -DskipTests'
            }
        }
        stage("Build docker image"){
            steps{
                script{
                    bat "docker build -t lexicography ."
                }
            }
         }
    }
}