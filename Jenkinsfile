pipeline {
    agent any

    environment {
        WORK_DIR = "/var/lib/jenkins/workspace/Game"
        IMAGE_NAME = "indie-gems"
        IMAGE_TAG       = "${BUILD_NUMBER}"
        CONTAINER_NAME = "indie-gems-container"
        PORT = "9676"   // External port for app
        DOCKERHUB_USER  = "pavansai33"
        DOCKER_CREDS    = "pavansai33"
        CONTAINER_PORT  = "80"
    }

    stages {
        stage('Checkout Code') {
            steps {
                dir("${WORK_DIR}") {
https://github.com/myself-8816/Indie_Gems_Portal.git

                }
            }
        }

        stage('Build Docker Image') {
    steps {
        dir("${WORK_DIR}") {
            sh '''
                # Remove old image if exists
                docker rmi -f ${DOCKERHUB_USER}/${IMAGE_NAME}:${IMAGE_TAG} || true

                # Build new image
                docker build -t ${DOCKERHUB_USER}/${IMAGE_NAME}:${IMAGE_TAG} .
            '''
        }
    }
}


         stage('DockerHub Login') {
            steps {
                withCredentials([usernamePassword(
                    credentialsId: "${DOCKER_CREDS}",
                    usernameVariable: 'DOCKER_USER',
                    passwordVariable: 'DOCKER_PASS'
                )]) {

                    sh """
                    echo \$DOCKER_PASS | docker login -u \$DOCKER_USER --password-stdin
                    """
                }
            }
        }

         stage('Push Image to DockerHub') {
            steps {
                sh """
                docker push ${DOCKERHUB_USER}/${IMAGE_NAME}:${IMAGE_TAG}
                """
            }
        }


        stage('Run Docker Container') {
    steps {
        dir("${WORK_DIR}") {
            sh '''
                docker rm -f ${CONTAINER_NAME} || true

                docker run -d \
                -p ${PORT}:${CONTAINER_PORT} \
                --name ${CONTAINER_NAME} \
                ${DOCKERHUB_USER}/${IMAGE_NAME}:${IMAGE_TAG}
            '''
        }
    }
}

          }
}



