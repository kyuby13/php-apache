env.DOCKER_REGISTRY = 'yubyanime'
env.DOCKER_IMAGE_NAME = 'php'
pipeline {
    agent any
    stages {
        stage('Git Pull from Github') {
            steps {
                git credentialsId: 'GitHub', url: 'https://github.com/kyuby13/php-apache.git'
            }
        }    
       stage('Build Docker Image') {
            steps {
                sh "docker build -t $DOCKER_REGISTRY/$DOCKER_IMAGE_NAME:${BUILD_NUMBER} ."
            }
        }              
        stage('Push Docker Image to Dockerhub') {
            steps {
                sh "docker push $DOCKER_REGISTRY/$DOCKER_IMAGE_NAME:${BUILD_NUMBER}"
            }
        } 
        
        stage('Deploy') {
            steps {
                 sh """
                    ssh root@13.250.104.66 "docker ps | grep php | xargs docker stop; docker pull $DOCKER_REGISTRY/$DOCKER_IMAGE_NAME:${BUILD_NUMBER}; docker container create --name php${BUILD_NUMBER} -p 8787:80 $DOCKER_REGISTRY/$DOCKER_IMAGE_NAME:${BUILD_NUMBER}; docker container start php${BUILD_NUMBER}; docker system prune -af"
                    """
             }
        }
          
        
       
    }
}
