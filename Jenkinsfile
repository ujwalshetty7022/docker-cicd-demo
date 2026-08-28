pipeline {

    agent any

    environment {
        IMAGE_NAME = "mywebsite"
        CONTAINER_NAME = "mywebsite-container"
    }

    stages {

        stage('Clone') {
            steps {
                echo 'Cloning source code...'

                git branch: 'main',
                    url: 'https://github.com/ujwalshetty7022/docker-cicd-demo.git'
            }
        }

        stage('Test') {
            steps {
                echo 'Testing application...'

                sh '''
                    test -f index.html
                    test -f Dockerfile
                '''
            }
        }

        stage('Docker Build') {
            steps {
                echo 'Building Docker image...'

                sh '''
                    docker build -t ${IMAGE_NAME}:latest .
                '''
            }
        }

        stage('Deploy') {
            steps {
                echo 'Deploying application...'

                sh '''
                    docker rm -f ${CONTAINER_NAME} 2>/dev/null || true

                    docker run -d \
                        --name ${CONTAINER_NAME} \
                        --restart unless-stopped \
                        -p 80:80 \
                        ${IMAGE_NAME}:latest
                '''
            }
        }
    }

    post {

        success {
            echo '================================='
            echo 'Deployment Successful!'
            echo '================================='
        }

        failure {
            echo '================================='
            echo 'Pipeline Failed!'
            echo '================================='
        }
    }
}