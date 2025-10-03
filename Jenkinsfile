pipeline {
  agent any
  stages {
    stage('Build') {
      steps {
        echo 'this is the first job'
        sh 'mvn compile'
      }
    }

    stage('Test') {
      steps {
        echo 'this is the second job'
        sh 'mvn clean test'
      }
    }

    stage('Package') {
      steps {
        echo 'this is the third job'
        sh '''# Truncate the GIT_COMMIT to the first 7 characters
GIT_SHORT_COMMIT=${echo $GIT_COMMIT | cut -c 1-7}

# set the version using maven
mvn versions:set-DnewVersion="$GIT_SHORT_COMMIT"
mvn versions:commit'''
        sh 'mvn package -DskipTests'
        archiveArtifacts '**/target*/jar'
      }
    }

  }
  tools {
    maven '3.9.6'
  }
  post {
    always {
      echo 'this pipeline has completed...'
    }

  }
}