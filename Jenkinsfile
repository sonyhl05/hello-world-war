pipeline {
    agent { label 'slave01' }

    environment {
        DOCKER_IMAGE = "sonyhl30/helloworld:${BUILD_NUMBER}"
        DOCKER_USERNAME = "your-username"
        DOCKER_PASSWORD = "your-password"
    }

    stages {
        stage('Checkout Code') {
            steps {
                echo 'Checking out code...'
                checkout scm
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    echo 'Building Docker image...'
                    sh "docker build -t $DOCKER_IMAGE ."
                }
            }
        }

        stage('Push Docker Image') {
            steps {
                script {
                    echo 'Logging into Docker registry...'
                    #sh "echo $DOCKER_PASSWORD | docker login your-docker-registry.com -u $DOCKER_USERNAME --password-stdin"
                    echo 'Pushing Docker image to registry...'
                    sh "docker push $DOCKER_IMAGE"
                }
            }
        }

        stage('Deploy on Server') {
            steps {
                script {
                    echo 'Pulling Docker image on deployment server...'
                    sh "ssh -i ~/.ssh/id_ed25519 root@13.203.156.71 'docker pull $DOCKER_IMAGE'"
                    sh "ssh -i ~/.ssh/id_ed25519 root@13.203.156.71 'docker stop my-container || true'"
                    sh "ssh -i ~/.ssh/id_ed25519 root@13.203.156.71 'docker rm my-container || true'"
                    sh "ssh -i ~/.ssh/id_ed25519 root@13.203.156.71 'docker run -d --name my-container -p 8080:8080 $DOCKER_IMAGE'"
                }
            }
        }
    }
}
