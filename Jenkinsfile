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
         stage("Publish to Nexus Repository Manager") {
                             steps {
                                 script {
                                     pom = readMavenPom file: "pom.xml";
                                     filesByGlob = findFiles(glob: "target/*.${pom.packaging}");
                                     echo "${filesByGlob[0].name} ${filesByGlob[0].path} ${filesByGlob[0].directory} ${filesByGlob[0].length} ${filesByGlob[0].lastModified}"
                                     artifactPath = filesByGlob[0].path;
                                     artifactExists = fileExists artifactPath;
                                     if(artifactExists) {
                                         echo "*** File: ${artifactPath}, group: ${pom.groupId}, packaging: ${pom.packaging}, version ${pom.version}";
                                         nexusArtifactUploader(
                                             nexusVersion: 3,
                                             protocol: HTTP,
                                             nexusUrl: NEXUS_URL,
                                             groupId: pom.groupId,
                                             version: pom.version,
                                             repository: NEXUS_REPOSITORY,
                                             credentialsId: NEXUS_CREDENTIAL_ID,
                                             artifacts: [
                                                 [artifactId: pom.artifactId,
                                                 classifier: '',
                                                 file: artifactPath,
                                                 type: pom.packaging],
                                                 [artifactId: pom.artifactId,
                                                 classifier: '',
                                                 file: "pom.xml",
                                                 type: "pom"]
                                             ]
                                         );
                                     } else {
                                         error "*** File: ${artifactPath}, could not be found";
                                     }
                                 }
                             }
                         }
    }
}