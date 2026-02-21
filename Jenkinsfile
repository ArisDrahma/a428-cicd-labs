pipeline {
    agent {
        docker {
            image 'maven:3.9.9-eclipse-temurin-21'
            args '-p 3000:3000'
        }
    }

    environment {
        APP_PORT = '3000'
    }

    stages {

        stage('Build') {
            steps {
                sh 'mvn clean package -DskipTests'
            }
        }

        stage('Test') {
            steps {
                sh 'mvn test'
            }
        }

        stage('Deploy') {
            steps {
                sh '''
                mkdir -p logs
                nohup java -jar target/*.jar --server.port=$APP_PORT > logs/app.log 2>&1 &
                '''
                echo "Aplikasi berjalan di http://localhost:3000"
                
                input message: 'Sudah selesai menggunakan aplikasi? Klik Proceed untuk menghentikan.'
                
                sh '''
                pkill -f 'java -jar'
                '''
            }
        }
    }
}