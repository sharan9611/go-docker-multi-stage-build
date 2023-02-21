pipeline {
    agent any
    environment {
        AWS_ACCOUNT_ID="461706885282"
        AWS_DEFAULT_REGION="us-east-1"
        IMAGE_REPO_NAME="demo"
        IMAGE_TAG="latest"
        REPOSITORY_URI = "${AWS_ACCOUNT_ID}.dkr.ecr.${AWS_DEFAULT_REGION}.amazonaws.com/${IMAGE_REPO_NAME}"
    }
   
    stages {
        
         stage('Logging into AWS ECR') {
            steps {
                script {
                sh "aws ecr-public get-login-password --region us-east-1 | docker login --username AWS --password-stdin public.ecr.aws/x8z8q6s2"
                }
                 
            }
        }
        
        stage('Cloning Git') {
            steps {
                checkout scmGit(branches: [[name: '*/test']], extensions: [], userRemoteConfigs: [[credentialsId: '23a41ef3-9c8b-41ef-ba7e-17c6fdcf5928', url: 'https://github.com/sharan9611/go-docker-multi-stage-build.git']])     
            }
        }
  
    // Building Docker images
    stage('Building image') {
      steps{
        script {
          dockerImage = "docker build -t demo ."
        }
      }
    }
   
    // Uploading Docker images into AWS ECR
    stage('Pushing to ECR') {
     steps{  
         script {
                sh "docker tag demo:latest public.ecr.aws/x8z8q6s2/demo:latest:$latest"
                sh "docker push public.ecr.aws/x8z8q6s2/demo:latest:${latest}"
          }
       }
    }
  }
}
