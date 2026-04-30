pipeline {
    agent any

    tools {
        maven 'Maven3'
        jdk 'JDK21'
    }

    stages {

        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/sandeep007766/spring-framework-petclinic.git'
            }
        }

        stage('Build') {
            steps {
                sh 'mvn clean package -DskipTests'
            }
        }

        stage('Run Application') {
            steps {
                sh '''
                    pkill -f "spring" || true
                    nohup mvn spring-boot:run -Dspring-boot.run.arguments=--server.port=8081 > app.log 2>&1 &
                '''
            }
        }
    }

    post {
        success {
            echo "App started successfully 🚀"
        }
        failure {
            echo "Build failed ❌"
        }
    }
}
