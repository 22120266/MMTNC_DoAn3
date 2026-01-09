pipeline {
    agent any

    environment {
        DOCKER_HUB_USER = "22120266" 
        IMAGE_NAME = "mmtnc-doan3"
        IMAGE_TAG = "latest"
        DOCKER_HUB_CREDS = 'docker-hub-credentials'
    }

    stages {
        stage('1. Git Pull') { 
            steps {
                checkout scm
                echo "Git pull completed successfully."
            }
        }

        stage('2. Build and Publish to Dockerhub') { 
            steps {
                script {
                    def customImage = docker.build("${DOCKER_HUB_USER}/${IMAGE_NAME}:${IMAGE_TAG}")

                    docker.withRegistry('', DOCKER_HUB_CREDS) {
                        customImage.push()
                    }
                }
            }
        }

        stage('3. Jenkins deploy the app to docker container') { 
            steps {
                script {
                    sh "docker rm -f ${IMAGE_NAME} || true"
                    
                    sh "docker run -d --name ${IMAGE_NAME} -p 8081:80 ${DOCKER_HUB_USER}/${IMAGE_NAME}:${IMAGE_TAG}"
                    
                    echo "Application deployed to port 8081."
                }
            }
        }
    }
}
