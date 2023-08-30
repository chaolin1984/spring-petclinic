def s3_bucketname
pipeline {
    agent any

    

    stages {
        stage('Git checkout') {
            steps{
                // Get source code from a GitHub repository
                
                git branch:'main', credentialsId:'github-cred', url:'git@github.com:chaolin1984/spring-petclinic.git'
            
            }
        }
        // CI
        stage('mvn install') {
            steps{
                dir("./") {
                    sh 'sudo apt-get -y install maven'
                }
            }
        }
        
        stage('Build') {
            steps{
                dir("./") {
                    sh './mvnw package'
                }
            }
        }
    
// CD

        stage("upload file to EC2"){
            steps {
                sshagent(credentials: ['jenkins-aws']) {
                  sh '''
                      scp -o StrictHostKeyChecking=no target/*.jar ubuntu@3.25.224.162:/home/ubuntu
                  '''
                }
            }
        }   
        
    }
}
