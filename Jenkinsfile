pipeline {
    agent any

    environment {
        IMAGE_NAME = "lab8-app"
        CONTAINER_NAME = "lab8-app"
    }

    stages {
        stage('🧾 Checkout Source Code') {
            steps {
                echo "📥 Cloning source from GitHub..."
                git branch: 'main', url: 'https://github.com/NguyenPhamCongSon/Lab8_DTDM.git'
            }
        }

        stage('🐳 Build Docker Image') {
            steps {
                script {
                    echo "🔨 Building Docker image..."
                    dockerImage = docker.build("${IMAGE_NAME}:${env.BUILD_ID}")
                }
            }
        }

        stage('🚀 Run Docker Container') {
            steps {
                script {
                    echo "🧹 Cleaning up old container (if any)..."
                    sh "docker stop ${CONTAINER_NAME} || true"
                    sh "docker rm ${CONTAINER_NAME} || true"

                    echo "🚀 Running new container..."
                    dockerImage.run("-d -p 5000:5000 --name ${CONTAINER_NAME}")
                }
            }
        }
    }

    post {
        success {
            echo '✅ CI/CD Pipeline completed successfully!'
        }
        failure {
            echo '❌ CI/CD Pipeline failed. Please check logs.'
        }
        always {
            echo "📦 Docker Image: ${IMAGE_NAME}:${env.BUILD_ID}"
        }
    }
}
