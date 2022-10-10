pipeline {
    agent any
    environment {
        DOCKER_IMAGE="daohungjs/jenkins-registry"
        DOCKER_TAG = "jenkins-${JOB_NAME}-${BUILD_NUMBER}"
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
            environment {
                DOCKER_PASSWORD=Daohung''
            }
            steps {
                withCredentials([usernamePassword(credentialsId: 'registry', 
                                                  usernameVariable: 'DOCKER_USERNAME' , 
                                                  passwordVariable: 'DOCKER_PASSWORD')]) 
                {
                    sh "echo ${DOCKER_PASSWORD} | docker login -u ${DOCKER_USERNAME} --password-stdin"
                }
                }
            }
        }
    }