pipeline{
    agent any
    tools{
        maven 'Maven 3.6.3'
    }
    environment {
        DOCKERHUB_PASSWORD = credentials('dh-pass')
        DOCKERHUB_USERNAME = credentials ('dh-user')
    }
    stages{
        stage('Build'){
            steps {
                echo 'String operator'
                sh 'java --version'
                sh 'mvn clean compile'
            }
        }
        stage('Test'){
            steps {
                sh 'mvn test'
            }
        }
        stage('Deploy'){
            steps {
                sh 'mvn package'
                sh 'docker --version'
                sh 'docker build -t bimz/string-operator .'
                sh 'docker login --username=${DOCKERHUB_USERNAME} --password=${DOCKERHUB_PASSWORD}'
                sh 'docker push bimz/string-operator'
            }
            post {
                success {
                    archiveArtifacts 'target/*.jar'
                }
            }
        }
    }
}