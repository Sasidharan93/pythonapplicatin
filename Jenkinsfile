pipeline {
    agent any

    environment {
        DOCKER_HUB_USER = 'sasidharan93'
        DOCKER_HUB_PASS = 'your-dockerhub-password'
        IMAGE_NAME = 'sasidharan93/pythonapplication'
        IMAGE_TAG = 'latest'
        CONTAINER_NAME = 'python-app'
        GIT_REPO = 'https://github.com/Sasidharan93/pythonapplicatin.git'
        GIT_BRANCH = 'main'
    }

    stages {
        stage('Clone Repository') {
            steps {
                script {
                    bat "rmdir /s /q repo || exit 0"
                    bat "git clone -b %GIT_BRANCH% %GIT_REPO% repo"
                }
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    bat "cd repo && docker build -t %IMAGE_NAME%:%IMAGE_TAG% ."
                }
            }
        }

        stage('Login to Docker Hub') {
            steps {
                script {
                    bat "echo %DOCKER_HUB_PASS% | docker login -u %DOCKER_HUB_USER% --password-stdin"
                }
            }
        }

        stage('Push Docker Image') {
            steps {
                script {
                    bat "docker push %IMAGE_NAME%:%IMAGE_TAG%"
                }
            }
        }

        stage('Run Python Code in Docker Container') {
            steps {
                script {
                    bat "docker run --rm --name %CONTAINER_NAME% %IMAGE_NAME%:%IMAGE_TAG%"
                }
            }
        }

        stage('Cleanup') {
            steps {
                script {
                    bat "docker rmi %IMAGE_NAME%:%IMAGE_TAG%"
                    bat "docker system prune -f"
                }
            }
        }
    }

    post {
        always {
            cleanWs()
        }
    }
}
