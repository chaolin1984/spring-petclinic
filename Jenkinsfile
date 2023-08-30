def s3_bucketname
pipeline {
    agent any

    

    stages {
        stage('Git checkout') {
            steps{
                // Get source code from a GitHub repository
                
                git branch:'main', credentialsId:'jenkins-node-private-key', url:'git@github.com:chaolin1984/spring-petclinic.git'
            
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

        stage("Upload"){
            steps{
                dir("./"){
                withAWS(region:"ap-southeast-2", credentials:"jenkins_aws"){
                    // s3Delete(bucket: "${env.UATS3BucketName}", path:'**/*')
                    s3Upload(bucket: "techscrum-frontend-jr10", workingDir:'build', includePathPattern:'**/*');
                    }
                } 
            }   
        }
    }
}
