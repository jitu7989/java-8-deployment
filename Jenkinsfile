pipeline {
    agent {
        docker {
            image 'openjdk:8-jdk'
            args '-v /root/.m2:/root/.m2 --privileged -v /var/run/docker.sock:/var/run/docker.sock'
        }
    }

    environment {
        JAVA_VERSION = '8'
    }

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/jitu7989/java-8-deployment.git'
            }
        }
        stage('Build') {
            steps {
                sh '''
                    echo "Building with Java ${JAVA_VERSION}"
                    java -version
                    mvn clean install
                '''
            }
        }
        stage('Package') {
            steps {
                sh 'mvn package -DskipTests'
                archiveArtifacts artifacts: 'target/*.jar', fingerprint: true
            }
        }

    }

post {
        always {
            echo 'Pipeline completed'
        }
        success {
            echo '✅ Build completed successfully!'
        }
        failure {
            echo '❌ Build failed!'
        }
        cleanup {
            cleanWs()
        }
    }
}