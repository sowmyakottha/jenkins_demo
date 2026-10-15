pipeline {
    agent {
        label 'SlaveA'  // Replace with your worker node's label
    }
    
    environment {
        DOCKER_IMAGE = 'sowmyakottha/my-app:latest'
        GIT_REPO = 'https://github.com/sowmyakottha/jenkins_demo.git'
    }

    stages {
        stage('Clone Source Code') {
            steps {
                git branch: 'main', url: "${GIT_REPO}"
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    def appImage = docker.build(DOCKER_IMAGE)
                }
            }
        }

        stage('Push Docker Image') {
            steps {
                script {
                    docker.withRegistry('lakshminagathrisowmya', 'dckr_pat_g4N1qFg7ZuXspU2FDVkfws7TsKs') {
                        docker.image(DOCKER_IMAGE).push()
                    }
                }
            }
        }

        stage('Deploy Application') {
            steps {
                script {
                    sh """
                    docker stop my-app || true
                    docker rm my-app || true
                    docker run -d --name my-app -p 5000:5000 ${DOCKER_IMAGE}
                    """
                }
            }
        }
    }

    post {
        always {
            echo 'Cleaning up workspace...'
            cleanWs()
        }
    }
}
