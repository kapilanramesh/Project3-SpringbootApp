pipeline {
    agent any

    environment {
        IMAGE_NAME = "springboot-war-app"
        CONTAINER_NAME = "springboot-tomcat-container"
    }

    stages {
        stage('Clone Code') {
            steps {
                git 'https://github.com/kapilanramesh/Project3-SpringbootApp'
            }
        }

        stage('Build WAR') {
            steps {
                sh 'mvn clean package'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh 'docker build -t $IMAGE_NAME .'
            }
        }

        stage('Stop Old Container') {
            steps {
                sh '''
                docker stop $CONTAINER_NAME || true
                docker rm $CONTAINER_NAME || true
                '''
            }
        }

        stage('Run New Container') {
            steps {
                sh '''
                docker run -d --name $CONTAINER_NAME -p 8081:8080 $IMAGE_NAME
                '''
            }
        }
    }
}


