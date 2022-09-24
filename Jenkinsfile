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
                    bat "docker build -t gomycode ."
                }
            }
         }
         stage("push docker image to dockerhub"){
            steps{
                withCredentials([string(credentialsId: 'dockerhubpassword', variable: 'mydockerhub')]) {
                bat "docker login -u theoldisback -p ${mydockerhub}"
                bat "docker tag gomycode theoldisback/gomycode:pipline"
                bat "docker push theoldisback/gomycode:pipline"

            }
            }
         }
    }
}