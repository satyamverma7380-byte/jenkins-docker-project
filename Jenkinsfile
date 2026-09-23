pipeline {
    agent {    label 'dev'}

    environment {
        DOCKER_IMAGE = "satyamverma7380-byte/jenkins-docker-project"
    }

    stages {

        stage('Clone Code') {
    steps {
        git branch: 'main',
            url: 'https://github.com/satyamverma7380-byte/jenkins-docker-project.git'
    }
}

        stage('Build Docker Image') {
            steps {
                sh '''
                    docker build -t ${DOCKER_IMAGE}:latest .
                '''
            }
        }

        stage('Docker Login') {
            steps {
                withCredentials([
                    usernamePassword(
                        credentialsId: 'dockerhub-creds',
                        usernameVariable: 'DOCKER_USERNAME',
                        passwordVariable: 'DOCKER_PASSWORD'
                    )
                ]) {
                    sh '''
                        echo "$DOCKER_PASSWORD" | docker login \
                        -u "$DOCKER_USERNAME" \
                        --password-stdin
                    '''
                }
            }
        }

        stage('Push Image') {
            steps {
                sh '''
                    docker push ${DOCKER_IMAGE}:latest
                '''
            }
        }

        stage('Run Container') {
            steps {
                sh '''
                    docker stop myapp || true
                    docker rm myapp || true

                    docker run -d \
                    --name myapp \
                    -p 8080:80 \
                    ${DOCKER_IMAGE}:latest
                '''
            }
        }
    }
}
