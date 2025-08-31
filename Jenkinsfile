pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/manojkimkhabwala/hello-world-app.git'
            }
        }
        
        stage('Build') {
            tools {
                maven 'Maven 3.8.4' // Use the name you gave in Global Tool Configuration
            }
            steps {
                sh 'mvn clean package'
            }
        }
        
        stage('Deploy to Tomcat') {
            steps {
                script {
                    withCredentials([sshUserPrivateKey(
                        credentialsId: 'ssh',
                        usernameVariable: 'TOMCAT_USER',
                        keyFileVariable: 'KEY_FILE'
                    )]) {
                        
                         // The key file variable now holds the path to the key.
                        sh "ssh -i ${KEY_FILE} -o StrictHostKeyChecking=no ${TOMCAT_USER}@54.80.24.66 'sudo systemctl stop tomcat && sudo rm -rf /opt/tomcat/webapps/hello-world-app.war && sudo rm -rf /opt/tomcat/webapps/hello-world-app/'"
                        sh "scp -i ${KEY_FILE} -o StrictHostKeyChecking=no 'target/hello-world-app.war' ${TOMCAT_USER}@54.80.24.66:/opt/tomcat/webapps/"
                        sh "ssh -i ${KEY_FILE} -o StrictHostKeyChecking=no ${TOMCAT_USER}@54.80.24.66 'sudo systemctl start tomcat'"                   
                    }
                }
            }
        }
    }
}
