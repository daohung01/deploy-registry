pipeline {
    agent any
    environment {
        DOCKER_IMAGE="daohungjs/jenkins-registry"
        DOCKER_TAG = "${GIT_BRANCH.tokenize('/').pop()}-${GIT_COMMIT.substring(0,7)}"
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
        stage("Push") {
            options {
                timeout(time: 10, unit: 'MINUTES')
            }
            steps {
                withDockerRegistry(credentialsId: 'Deploy-registry', url: 'https://index.docker.io/v1/') {
                    sh "docker push ${DOCKER_IMAGE}:${DOCKER_TAG}"
                    sh "docker push ${DOCKER_IMAGE}:latest"
                }
                //withCredentials([usernamePassword(credentialsId: 'Deploy-registry', usernameVariable: 'USERNAME', passwordVariable: 'PASSWORD')]) {
                    //sh "echo ${PASSWORD} | docker login -u ${USERNAME} --password-stdin"
                    //sh "docker push ${DOCKER_IMAGE}:${DOCKER_TAG}"
                    //sh "docker push ${DOCKER_IMAGE}:latest"
                }
            }
        }

    }