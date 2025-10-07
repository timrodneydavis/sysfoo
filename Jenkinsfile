pipeline {
  agent none
  stages {
    stage('Build') {
      agent {
        docker {
          image 'maven:3.9.6-eclipse-temurin-17-alpine'
        }

      }
      steps {
        echo 'this is the first job'
        sh 'mvn compile'
      }
    }

    stage('Test') {
      agent {
        docker {
          image 'maven:3.9.6-eclipse-temurin-17-alpine'
        }

      }
      steps {
        echo 'this is the second job'
        sh 'mvn clean test'
      }
    }

    stage('Package') {
      parallel {
        stage('Package') {
          agent {
            docker {
              image 'maven:3.9.6-eclipse-temurin-17-alpine'
            }

          }
          steps {
            echo 'this is the third job'
            sh '''# Truncate the GIT_COMMIT to the first 7 characters
GIT_SHORT_COMMIT=$(echo $GIT_COMMIT | cut -c 1-7)

# set the version using maven
mvn versions:set -DnewVersion=$GIT_SHORT_COMMIT
mvn versions:commit
'''
            sh 'mvn package -DskipTests'
            archiveArtifacts '**/target/*.jar'
          }
        }

        stage('Docker B&P') {
          steps {
            script {
              docker.withRegistry('https://index.docker.io/v1/', 'dockerlogin') {
                def commitHash = env.GIT_COMMIT.take(7)
                def dockerImage = docker.build("timrodney/sysfoo:${commitHash}", "./")
                dockerImage.push()
                dockerImage.push("latest")
                dockerImage.push("dev")
              }
            }

          }
        }

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