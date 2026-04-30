pipeline {
    agent any

    tools {
        maven 'Maven3'
        jdk 'JDK21'
    }

    stages {

        stage('Checkout') {
            steps {
                git branch: 'main',
                url: 'https://github.com/sandeep007766/spring-framework-petclinic.git'
            }
        }

        stage('Build') {
            steps {
                sh 'mvn clean package -DskipTests'
            }
        }

        stage('Deploy & Run') {
            steps {
                sh '''
                    echo "Stopping old processes..."
                    pkill -f petclinic || true
                    pkill -f spring || true
                    pkill -f java || true

                    echo "Starting Spring Boot application using Maven..."

                    nohup mvn spring-boot:run \
                    -Dspring-boot.run.arguments=--server.port=8081 \
                    > app.log 2>&1 &

                    sleep 15

                    echo "Checking if app is running..."
                    ps -ef | grep spring
                    netstat -tlnp | grep 8081 || true
                '''
            }
        }
    }

    post {
        success {
            echo "✅ SUCCESS - App should be running on http://<EC2-IP>:8081"
        }

        failure {
            echo "❌ FAILED - check app.log"
        }
    }
}
