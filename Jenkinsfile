pipeline {
    agent any

    environment {
        DOCKER_CREDS = credentials('docker-hub')
    }

    stages {
        stage('Clone Repo') {
            steps {
                git branch: 'main', url: 'https://github.com/Meghana2417/OrderService.git'
            }
        }

        stage('Docker Build') {
            steps {
                sh "docker build --no-cache -t meghana1724/orderservice:latest ."
            }
        }

        stage('Docker Login') {
            steps {
                sh 'echo "$DOCKER_CREDS_PSW" | docker login -u "$DOCKER_CREDS_USR" --password-stdin'
            }
        }

        stage('Docker Push') {
            steps {
                sh "docker push meghana1724/orderservice:latest"
            }
        }

        stage('Deploy') {
            steps {
                sh '''
                docker stop orderservice || true
                docker rm orderservice || true

                docker pull meghana1724/orderservice:latest

                docker run -d --name orderservice -p 8005:8005 meghana1724/orderservice:latest
                '''
            }
        }
    }
}
