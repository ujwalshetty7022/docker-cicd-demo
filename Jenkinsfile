pipeline {

    agent none

    environment {
        IMAGE_NAME = "mywebsite"
        CONTAINER_NAME = "mywebsite-container"
    }

    stages {

        stage('Clone') {

            agent {
                label 'built-in'
            }

            steps {
                echo '================================='
                echo 'Cloning source code on Controller'
                echo '================================='

                git branch: 'main',
                    url: 'https://github.com/ujwalshetty7022/docker-cicd-demo.git'
            }
        }

        stage('Test') {

            agent {
                label 'built-in'
            }

            steps {
                echo '================================='
                echo 'Testing application on Controller'
                echo '================================='

                sh '''
                    test -f index.html
                    test -f Dockerfile
                '''

                echo 'Application tests passed!'
            }
        }

        stage('Deploy Agent 1') {

            agent {
                label 'deploy-agent-1'
            }

            steps {
                echo '================================='
                echo 'Deploying to Agent 1'
                echo 'IP: 172.31.1.111'
                echo '================================='

                sh '''
                    docker build -t ${IMAGE_NAME}:latest .

                    docker rm -f ${CONTAINER_NAME} 2>/dev/null || true

                    docker run -d \
                        --name ${CONTAINER_NAME} \
                        --restart unless-stopped \
                        -p 80:80 \
                        ${IMAGE_NAME}:latest
                '''
            }
        }

        stage('Deploy Agent 2') {

            agent {
                label 'deploy-agent-2'
            }

            steps {
                echo '================================='
                echo 'Deploying to Agent 2'
                echo 'IP: 172.31.1.74'
                echo '================================='

                sh '''
                    docker build -t ${IMAGE_NAME}:latest .

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
            echo 'Distributed Deployment Successful!'
            echo '================================='
        }

        failure {
            echo '================================='
            echo 'Pipeline Failed!'
            echo '================================='
        }
    }
}
```
