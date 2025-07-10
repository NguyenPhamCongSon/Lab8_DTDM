pipeline {
    agent any

    stages {
        stage('📥 Checkout Code') {
            steps {
                git branch: 'main', url: 'https://github.com/NguyenPhamCongSon/Lab8_DTDM.git'
            }
        }

        stage('🐳 Build Image') {
            steps {
                sh 'docker build -t lab8-app .'
            }
        }

        stage('🚀 Deploy') {
            steps {
                script {
                    sh 'docker stop lab8-app || true'
                    sh 'docker rm lab8-app || true'
                    sh 'docker run -d -p 5000:5000 --name lab8-app --restart unless-stopped lab8-app'
                    sleep(5)
                    sh 'curl -f http://localhost:5000 || exit 1'
                }
            }
        }
    }

    post {
        success {
            echo '✅ THÀNH CÔNG: Ứng dụng đang chạy tại http://localhost:5000'
        }
        failure {
            sh 'docker logs lab8-app || true'
            echo '❌ LỖI: Xem log container phía trên'
        }
    }
}
