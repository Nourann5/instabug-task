pipeline {
    agent any
    
    environment {
        DOCKER_IMAGE_NAME = "nouranahmed5/instabug-app-task"
        DOCKERFILE_PATH = "instabug-task/"
        DOCKER_REPO_CREDENTIALS = "docker-hub"
    }
    
    stages {
        stage('Build and Pushing Docker Image') {
            steps {
                script {
                    def dockerImageTag = "${env.BUILD_NUMBER}"
                    
                    // Pull Docker repository credentials
                    withCredentials([usernamePassword(credentialsId: env.DOCKER_REPO_CREDENTIALS, passwordVariable: 'DOCKER_REPO_PASSWORD', usernameVariable: 'DOCKER_REPO_USERNAME')]) {
                        
                        // Build Docker image and tag it
                        sh "docker build -t ${env.DOCKER_IMAGE_NAME}:${dockerImageTag} ${env.DOCKERFILE_PATH}"
                        
                        // Login to Docker repository
                        sh "docker login -u ${env.DOCKER_REPO_USERNAME} -p ${DOCKER_REPO_PASSWORD}"
                        
                        // Push Docker image to repository
                        sh "docker push ${env.DOCKER_IMAGE_NAME}:${dockerImageTag}"
                        
                        // Remove local Docker image
                        sh "docker rmi ${env.DOCKER_IMAGE_NAME}:${dockerImageTag}"
                    }
                
                }
            }
        }
    }
    post {
           always {
               script {
         def buildStatus = currentBuild.result
                   def buildNumber = currentBuild.number
                   def buildUrl = "${env.BUILD_URL}"
                   def buildCommit = "${env.GIT_COMMIT}"
                   if (buildStatus == 'SUCCESS') {
                       slackSend(
                           channel: '#project', 
                           message: "Build ${buildNumber} succeeded! \n" +
                                    "Build URL: ${buildUrl}"
                       )
                   } else {
                       slackSend(
                           channel: '#project', 
                           message: "Build ${buildNumber} failed! \n" +
                                    "Build URL: ${buildUrl}"
                       )
                   }

               }

           }
       }
}