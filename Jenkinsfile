pipeline {
    agent any

    environment {
        DOCKER_IMAGE = "springboot-war-tomcat"
        CONTAINER_NAME = "springboot-war-container"
    }

    stages {
        stage('Clone Repo') {
            steps {
                git 'https://github.com/YOUR_USERNAME/springboot-war-app.git'
            }
        }

        stage('Build WAR with Maven') {
            steps {
                sh 'mvn clean package'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh '''
                    docker build -t $DOCKER_IMAGE .
                '''
            }
        }

        stage('Run WAR in Tomcat Container') {
            steps {
                sh '''
                    docker rm -f $CONTAINER_NAME || true
                    docker run -d --name $CONTAINER_NAME -p 8081:8080 $DOCKER_IMAGE
                '''
            }
        }
    }
}
