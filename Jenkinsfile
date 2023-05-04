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
  }  
}      