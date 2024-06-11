pipeline {
    agent any

    environment {
        // Define environment variables
        MAVEN_HOME = tool name: 'maven', type: 'maven'
        NEXUS_URL = 'http://192.168.31.142:8081/#admin/repository/repositories:maven-releases'
        NEXUS_REPO = 'maven-releases'
        NEXUS_CREDENTIALS_ID = 'nexus'
        TOMCAT_URL = 'http://192.168.31.142:8080'
        TOMCAT_CREDENTIALS_ID = 'tomcat-credentials'
    }

    stages {
        stage('Checkout') {
            steps {
                // Checkout code from version control
                 sh "git clone https://github.com/VinayKumarKonduru/javaproject.git"
            }
        }

        stage('Build') {
            steps {
                // Build the Maven project
                sh "${MAVEN_HOME}/bin/mvn -f /var/lib/jenkins/workspace/myproject/javaproject/simple-webapp/pom.xml clean package"
            }
        }


        stage('Deploy to Tomcat') {
            steps {
                // Deploy the artifact to Tomcat
                   withCredentials([string(credentialsId: 'tomcatserver', variable: 'pass')]) {
                    sh "sshpass -p ${pass} scp -o StrictHostKeyChecking=no /var/lib/jenkins/workspace/myproject/javaproject/simple-webapp/target/simple-webapp.war vkumar@192.168.31.142:/opt/tomcat/webapps"

                }
            }
        }
    }

    post {
        success {
            echo 'Pipeline completed successfully.'
        }
        failure {
            echo 'Pipeline failed.'
   }
  }
}
