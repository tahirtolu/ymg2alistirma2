pipeline {
    agent any

    environment {
        FRONTEND_IMAGE = 'frontend-image'
        FRONTEND_CONTAINER = 'frontend-container'
        BACKEND_IMAGE = 'backend-image'
        BACKEND_CONTAINER = 'backend-container'
        PATH = "C:\\Program Files\\Docker\\Docker\\resources\\bin;${env.PATH}"
    }

    triggers {
        githubPush()
    }

    stages {
        stage('Repo Klonla') {
            steps {
                git url: 'https://github.com/tahirtolu/ymg2alistirma2.git', branch: 'main'
            }
        }

        stage('Frontend Docker Image Oluştur') {
            steps {
                echo "Frontend Docker Image oluşturuluyor..."
                bat "docker build -t %FRONTEND_IMAGE% ./frontend"
            }
        }

        stage('Backend Docker Image Oluştur') {
            steps {
                echo "Backend Docker Image oluşturuluyor..."
                bat "docker build -t %BACKEND_IMAGE% ./backend"
            }
        }

        stage('Eski Frontend Container Durdur') {
            steps {
                echo "Eski frontend container durduruluyor..."
                bat "docker rm -f %FRONTEND_CONTAINER% || exit 0"
            }
        }

        stage('Eski Backend Container Durdur') {
            steps {
                echo "Eski backend container durduruluyor..."
                bat "docker rm -f %BACKEND_CONTAINER% || exit 0"
            }
        }

        stage('Yeni Frontend Container Oluştur') {
            steps {
                echo "Yeni frontend container oluşturuluyor..."
                bat "docker run -d --name %FRONTEND_CONTAINER% -p 6060:80 %FRONTEND_IMAGE%"
            }
        }

        stage('Yeni Backend Container Oluştur') {
            steps {
                echo "Yeni backend container oluşturuluyor..."
                // Backend portunu backend uygulamana göre değiştir
                bat "docker run -d --name %BACKEND_CONTAINER% -p 8081:8080 %BACKEND_IMAGE%"
            }
        }
    }

    post {
        success {
            echo "Yayın başarılı!"
        }
        failure {
            echo "Pipeline başarısız oldu!"
        }
    }
}
