pipeline {
  
  agent any
  
  tools {
    maven 'maven-3.8.6' 
  }
  
  stages {
    
    stage ("compile code") {
      steps {
        sh 'mvn --version'
        sh 'mvn compile'
      }
    }
    
    stage ("unit test") {
      steps {
        sh 'mvn test'
      }
      
      post {
        always {
          junit '**target/surefire-reports/*.xml'
        }
      }
    }
      
    stage ("build package") {
      steps {
        sh 'mvn package'
      }
      post {
        always {
          archiveArtifacts artifacts: '**/target/*.jar', onlyIfSuccessful: true
        }
      }
    }
    
    stage ("deploy code") {
      steps {
        sh 'java -jar target/*.jar'
      }
    }
    
    stage ("Upload artifact to S3") {
      steps {
        s3Upload entries: [[bucket: 'jenkins-java-maven-artifact', excludedFile: '', flatten: false, gzipFiles: false, keepForever: false, managedArtifacts: false, noUploadOnFailure: true, selectedRegion: 'us-east-1', showDirectlyInBrowser: false, sourceFile: 'target/*.jar', storageClass: 'STANDARD', uploadFromSlave: false, useServerSideEncryption: false]], pluginFailureResultConstraint: 'FAILURE', profileName: 's3-artifact', userMetadata: []
      }
    }
  }
}
      
