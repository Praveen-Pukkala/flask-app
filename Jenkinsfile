pipeline {
    agent any

    environment {
        DOCKER_HUB_CREDENTIALS = credentials('docker-hub') // Use your Jenkins credentials ID for Docker Hub
        GITHUB_CREDENTIALS = credentials('github-credentials') // Use your GitHub credentials ID
    }

    stages {
        stage('Clone Repository') {
            steps {
                git branch: 'main', 
                    credentialsId: 'github-credentials', 
                    url: 'https://github.com/Praveen-Pukkala/flask-app.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    sh 'docker build -t flask-app .'
                }
            }
        }

        stage('Push to Docker Hub') {
            steps {
                script {
                    sh 'echo $DOCKER_HUB_CREDENTIALS_PSW | docker login -u $DOCKER_HUB_CREDENTIALS_USR --password-stdin'
                    sh 'docker tag flask-app praveenpukkala/flask-app:latest'
                    sh 'docker push praveenpukkala/flask-app:latest'
                }
            }
        }

        stage('Deploy Container') {
            steps {
                script {
                    sh 'docker run -d -p 5000:5000 praveenpukkala/flask-app:latest'
                }
            }
        }
    }
}
