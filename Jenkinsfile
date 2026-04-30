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

        stage('Deploy') {
            steps {
                sh '''
                    echo "Stopping old application if running..."
                    pkill -f 'java -jar' || true

                    echo "Starting new application on port 8081..."
                    nohup java -jar target/*.jar --server.port=8081 > app.log 2>&1 &
                '''
            }
        }
    }

    post {
        success {
            echo 'Pipeline executed successfully 🎉'
        }

        failure {
            echo 'Pipeline failed ❌ Check logs'
        }
    }
}
