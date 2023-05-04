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
    }  
}      