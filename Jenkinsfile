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
                    echo "Stopping Tomcat..."
                    sudo systemctl stop tomcat9 || true

                    echo "Removing old deployment..."
                    sudo rm -rf /var/lib/tomcat9/webapps/petclinic*
                    
                    echo "Copying new WAR..."
                    sudo cp target/petclinic.war /var/lib/tomcat9/webapps/

                    echo "Starting Tomcat..."
                    sudo systemctl start tomcat9

                    sleep 15

                    echo "Checking Tomcat status..."
                    sudo systemctl status tomcat9
                '''
            }
        }
    }

    post {
        success {
            echo "✅ SUCCESS → http://<EC2-IP>:8081/petclinic"
        }
        failure {
            echo "❌ FAILED → Check Tomcat logs"
        }
    }
}
