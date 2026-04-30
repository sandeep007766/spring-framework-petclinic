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
                    echo "Stopping old application..."
                    pkill -f petclinic || true
                    pkill -f java || true

                    echo "Starting application..."

                    nohup java -jar target/petclinic.war \
                    --server.port=8081 \
                    --server.address=0.0.0.0 \
                    > app.log 2>&1 &

                    sleep 10

                    echo "Verifying process..."
                    ps -ef | grep java
                    netstat -tlnp | grep 8081 || true
                '''
            }
        }
    }

    post {
        success {
            echo "✅ Pipeline SUCCESS - Application should be running on port 8081"
        }

        failure {
            echo "❌ Pipeline FAILED - check logs"
        }
    }
}
