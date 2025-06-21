pipeline {
    agent any

     environment {
               REPO_URL = 'https://github.com/AsiFmahmud10/jenkins-setup'
               APP_NAME = 'app-service'
               IMAGE_TAG = 'dev'
         }

    stages {

        stage('Clone Repository') {
            steps {
                git url: "${REPO_URL}", branch: 'main'
            }
        }

        stage('Build with Maven') {
            steps {
                sh 'mvn clean package -DskipTests'
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    sh "docker rmi ${APP_NAME}:${IMAGE_TAG} || true"
                    sh "docker build -t ${APP_NAME}:${IMAGE_TAG} ."
                }
            }
        }

        stage('Run Docker Container') {
            steps {
                script {
                    sh "docker rm -f ${APP_NAME} || true"
                    sh "docker run -d --name ${APP_NAME} -p 9091:9090 ${APP_NAME}:${IMAGE_TAG}"
                }
            }
        }
    }

    post {
        success {
            echo 'Pipeline executed successfully.'
        }
        failure {
            echo 'Something went wrong!'
        }
    }
}




