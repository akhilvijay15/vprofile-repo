pipeline {
  agent any
  tools {
      maven "MAVEN-3.9.1"
      jdk "OracleJDK8"
  }
  stages {
       stage('Fetch code'){
          steps {
              git branch:'main', url: 'https://github.com/akhilvijay15/vprofile-repo.git'
          }
       }

       stage('Build'){
          steps{
             sh 'mvn install -DskipTests'
          }
          post {
             success {
                  echo 'Now Archiving it...'
                  archiveArtifacts artifacts: '**/target/*.war'
             }
          }
       }
       stage('UNIT TEST'){
          steps{
             sh 'mvn test'
          }
        }

        stage('Checkstyle Analysis'){
            steps{
                sh 'mvn checkstyle:checkstyle'
            }
        }

        stage('Sonar Analysis'){
            environment{
                scannerHome = tool 'sonar4.8'
            }
            steps{
                 withSonarQubeEnv('sonar') {
               sh '''${scannerHome}/bin/sonar-scanner -Dsonar.projectKey=vprofile \
                   -Dsonar.projectName=vprofile \
                   -Dsonar.projectVersion=1.0 \
                   -Dsonar.sources=src/ \
                   -Dsonar.java.binaries=target/test-classes/com/visualpathit/account/controllerTest/ \
                   -Dsonar.junit.reportsPath=target/surefire-reports/ \
                   -Dsonar.jacoco.reportsPath=target/jacoco.exec \
                   -Dsonar.java.checkstyle.reportPaths=target/checkstyle-result.xml'''
            }
        }
    }  
stage("Quality Gate") {
            steps {
                timeout(time: 1, unit: 'HOURS') {
                    // Parameter indicates whether to set pipeline to UNSTABLE if Quality Gate fails
                    // true = set pipeline to UNSTABLE, false = dont
                    waitForQualityGate abortPipeline: true

               }  
           }
        }
        stage("UploadArtifact"){
            steps{
                nexusArtifactUploader(
                nexusVersion: 'nexus3',
                protocol: 'http',
                nexusUrl: '52.90.145.27:8081',
                groupId: 'QA',
                version:  "${env.BUIL_ID}-${env.BUIL_TIMESTAMP}",
                repository: 'vprofile-repo',
                credentialsId: 'nexuslogin',
                artifacts: [
                [artifactId: 'vproapp',
                classifier: '',
                file: 'target/vprofile-v2.war',
                type: 'war'] 
                ]
                )
                
            }
       } 
    }
}                     