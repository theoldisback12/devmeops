pipline {
    agent any
    tools {
        maven 'maven'
    }
stages {
        stage('Build') {
            steps {
                bat 'mvn clean install -Dmaven.test.skip=true'
            }
        }
        stage('Testing'){
            steps{
                //sonarqube
                bat 'mvn test  -DskipTests'
            }
        }
        }
       }