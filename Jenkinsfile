pipeline {
    agent any

    environment {
        DOCKERHUB_CREDENTIALS = credentials('dockerhubCreds')  
        GITHUB_CREDENTIALS = credentials('githubToken')
        IMAGE_NAME = 'arshah22/app'  // DockerHub repo/image name
        CONTAINER_NAME = 'node_curr'  // You can rename as needed
    }

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main',credentialsId:  "${GITHUB_CREDENTIALS}", url: 'https://github.com/AdeyRatnaShah/jenkinsPrac.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    docker.build("${IMAGE_NAME}:latest")
                }
            }  
        }

        stage('Push to DockerHub') {
            steps {
                script {
                    docker.withRegistry('https://index.docker.io/v1/', DOCKERHUB_CREDENTIALS) {
                        docker.image("${IMAGE_NAME}:latest").push()
                    }
                }
            }
        }

        stage('Deploy Container') {
            steps {
                script {
                    sh '''
                        docker stop ${CONTAINER_NAME} || true
                        docker rm ${CONTAINER_NAME} || true
                        docker run -d --name ${CONTAINER_NAME} -p 3000:3000 ${IMAGE_NAME}:latest
                    '''
                }
            }
        }
    }
}
