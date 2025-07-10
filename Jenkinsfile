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
                    // Đảm bảo Dockerfile có trong repo
                    docker.build("${IMAGE_NAME}:${env.BUILD_ID}", ".")
                }
            }
        }

        stage('🚀 Run Docker Container') {
            steps {
                script {
                    echo "🧹 Cleaning up old container (if any)..."
                    sh(script: "docker stop ${CONTAINER_NAME} || true", returnStdout: false)
                    sh(script: "docker rm ${CONTAINER_NAME} || true", returnStdout: false)

                    echo "🚀 Running new container..."
                    sh "docker run -d -p 5000:5000 --name ${CONTAINER_NAME} ${IMAGE_NAME}:${env.BUILD_ID}"
                }
            }
        }
    }

    post {
        always {
            echo "📦 Docker Image: ${IMAGE_NAME}:${env.BUILD_ID}"
            echo "🛑 Stopping and removing container..."
            sh "docker stop ${CONTAINER_NAME} || true"
            sh "docker rm ${CONTAINER_NAME} || true"
        }
        success {
            echo '✅ CI/CD Pipeline completed successfully!'
        }
        failure {
            echo '❌ CI/CD Pipeline failed. Please check logs.'
        }
    }
}
