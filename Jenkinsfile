pipeline {
    agent any
    environment {
        DOCKER_HUB_USER = "22120266"
        IMAGE_NAME = "do-an-3-app"
        DOCKER_HUB_CREDS = 'docker-hub-credentials'
    }
    stages {
        stage('1. Git Pull') { steps { checkout scm } } [cite: 28]
        stage('2. Build & Publish') { 
            steps {
                script {
                    def app = docker.build("${DOCKER_HUB_USER}/${IMAGE_NAME}") [cite: 30]
                    docker.withRegistry('', DOCKER_HUB_CREDS) { app.push("latest") } [cite: 30]
                }
            }
        }
        stage('3. Deploy') { 
            steps {
                sh "docker rm -f ${IMAGE_NAME} || true"
                sh "docker run -d --name ${IMAGE_NAME} -p 8081:80 ${DOCKER_HUB_USER}/${IMAGE_NAME}:latest" [cite: 34]
            }
        }
    }
}
