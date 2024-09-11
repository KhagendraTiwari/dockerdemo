pipeline
{
    agent any
   
    stages{
        stage('build maven'){
            steps{
                checkout scmGit(branches: [[name: '*/main']], extensions: [], userRemoteConfigs: [[url: 'https://github.com/KhagendraTiwari/dockerdemo.git']])
               
            
                
            }
        }
        stage("build docker image"){
            steps{
                script{
                     sh 'docker build -t mydockerimg .' 
                }
            }
        }
         stage('Save Docker Image') {
            steps {
                script {
                    // Save the Docker image as a tar file
                    sh "docker save -o mydockerimg.tar mydockerimg"
                }
            }
        }
        stage('Transfer Docker Image to EC2') {
            steps {
                script {
                    // Transfer the Docker image tar file to the Ubuntu EC2 instance
                    sshagent(credentials: root-ssh-key) {
                        sh "scp -o StrictHostKeyChecking=no mydockerimg.tar root@3.88.228.221:/root/"
                    }
                }
            }
        }
    }
       
}
