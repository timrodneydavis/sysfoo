pipeline{

    agent any
// uncomment the following lines by removing /* and */ to enable
   tools{
       maven 'Maven 3.9.6' 
    }   

    stages{
        stage('Build'){
            steps{
                echo 'this is the first job'
                sh 'mvn compile'
            }
        }
        stage('Test'){
            steps{
                echo 'this is the second job'
                sh 'mvn clean test'
            }
        }
        stage('Package'){
            steps{
                echo 'this is the third job'
                sh 'mvn package -DskipTests'
            }
        }
    }
    
    post{
        always{
            echo 'this pipeline has completed...'
        }
        
    }
    
}
