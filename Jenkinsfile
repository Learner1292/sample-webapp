pipeline {
    agent { label 'worker' }
    
    tools {
        jdk 'Java 17'
        maven 'Maven 3.8.9'
    }

    environment {
        WAR_NAME = 'sample-webapp.war'
        DEPLOY_DIR = '/var/lib/tomcat10/webapps'
    }

    triggers {
        githubPush()
    }

    stages {
        stage('Clone Code') {
            steps {
                git branch: 'master', url: 'https://github.com/Learner1292/sample-webapp.git'
            }
        }

        stage('Build WAR') {
            steps {
                sh 'mvn clean package'
            }
        }

        stage('Deploy to Tomcat') {
            steps {
                sh """
                cp target/${WAR_NAME} ${DEPLOY_DIR}/
                sudo systemctl restart tomcat10
                """
            }
        }
    }

    post {
        success {
            echo 'Deployment successful!'
        }
        failure {
            echo 'Deployment failed!'
        }
    }
}
