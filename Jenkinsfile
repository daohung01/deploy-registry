pipeline {
    agent any
    environment {
        DOCKER_IMAGE="daohungjs/jenkins-registry"
        DOCKER_TAG = "${BUILD_NUMBER}"
    }
    stages {
        stage("Build") {
            options {
                timeout(time: 10, unit: 'MINUTES')
            }
            steps {
                sh '''
                docker build -t ${DOCKER_IMAGE}:${DOCKER_TAG} .
                docker tag ${DOCKER_IMAGE}:${DOCKER_TAG} ${DOCKER_IMAGE}:latest
                '''
                //clean to save disk
                //sh "docker image rm ${DOCKER_IMAGE}:${DOCKER_TAG}"
                //sh "docker image rm ${DOCKER_IMAGE}:latest"
                //sh "docker rmi nginx:1.13.9-alpine"
            }
        }
    }
}