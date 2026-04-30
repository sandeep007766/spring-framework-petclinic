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

        stage('Deploy to Tomcat') {
            steps {
                sh '''
                    echo "Deploying WAR to Tomcat..."

                    sudo cp target/petclinic.war /var/lib/tomcat9/webapps/

                    echo "Deployment completed"
                '''
            }
        }
    }

    post {
        success {
            echo "✅ SUCCESS - Open http://<EC2-IP>:8080/petclinic"
        }
        failure {
            echo "❌ FAILED - check logs"
        }
    }
}
