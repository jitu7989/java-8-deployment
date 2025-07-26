pipeline {
    agent {
        docker {
            image 'openjdk:8-jdk'
            args '-v /root/.m2:/root/.m2'
        }
    }

    environment {
        JAVA_VERSION = '8'
    }

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/your-company/your-java-service.git'
            }
        }
        stage('Build') {
            steps {
                sh '''
                    echo "Building with Java ${JAVA_VERSION}"
                    java -version
                    ./mvnw clean compile
                '''
            }
        }
        stage('Package') {
            steps {
                sh './mvnw package -DskipTests'
                archiveArtifacts artifacts: 'target/*.jar', fingerprint: true
            }
        }

    }

    post {
        cleanup {
            cleanWs()
        }
    }
}