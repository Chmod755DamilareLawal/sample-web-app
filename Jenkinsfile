pipeline {
    agent any
    environment {
        VERSION = "${env.BUILD_ID}"
        AWS_ACCOUNT_ID="4532803019085"
        AWS_DEFAULT_REGION="us-east-1"
        IMAGE_REPO_NAME="damilare-docker"
        IMAGE_TAG= "${env.BUILD_ID}"
        REPOSITORY_URI = "453280301908.dkr.ecr.us-east-1.amazonaws.com/damilare-docker"
    }
  stages {
      stage('Git checkout') { 
          steps { 
              git branch: 'main', credentialsId: '', url: 'https://github.com/Chmod755DamilareLawal/sample-web-app.git'
              }
        }
      stage('logging into AWS ECR') {
                  environment {
                      AWS_ACCESS_KEY_ID = credentials('aws_access_key_id')
                        AWS_SECRET_ACCESS_KEY = credentials('aws_secret_access_key')
            }
              steps {  
                script{
                    sh """aws ecr get-login-password --region ${AWS_DEFAULT_REGION} | docker login --username AWS --password-stdin ${AWS_ACCOUNT_ID}.dkr.ecr.${AWS_DEFAULT_REGION}.amazonaws.com"""
                }
              }
      }
      
          stage('Building image') {
            steps{
              script {
                dockerImage = docker.build "${IMAGE_REPO_NAME}:${IMAGE_TAG}"
               }
            }
          }
        
          stage('Pushing to ECR') {
              steps{
                  script {
                    //sh """docker tag ${IMAGE_REPO_NAME}:${IMAGE_TAG} ${REPOSITORY_URI}:$IMAGE_TAG"""
                    sh """docker push ${AWS_ACCOUNT_ID}.dkr.ecr.${AWS_DEFAULT_REGION}.amazonaws.com/${IMAGE_REPO_NAME}:${IMAGE_TAG}"""
                         
            }
        }
   }      
  }     
}
