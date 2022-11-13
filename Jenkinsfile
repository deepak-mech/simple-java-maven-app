pipeline {
  
  stages {
    
    stage ("compile code") {
      steps {
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
  }
}
      
