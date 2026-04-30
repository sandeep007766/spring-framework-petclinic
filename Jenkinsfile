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
                sh '''
                    echo "Building project..."
                    mvn clean package -DskipTests
                '''
            }
        }

        stage('Deploy') {
            steps {
                sh '''
                    echo "Stopping old application..."
                    pkill -f java || true

                    echo "Deploying WAR file..."

                    JAR_FILE=$(find target -name "petclinic.war" | head -n 1)

                    echo "Found artifact: $JAR_FILE"

                    nohup java -jar $JAR_FILE --server.port=8081 > app.log 2>&1 &
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
