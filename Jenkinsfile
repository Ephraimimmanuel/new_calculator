pipeline {
    agent any

    tools {
        maven 'Maven'
    }

    stages {

        stage('Clone') {
            steps {
                git branch: 'main',
                url: 'https://github.com/Ephraimimmanuel/new_calculator.git'
            }
        }

        stage('Build') {
            steps {
                bat 'mvn clean install'
            }
        }

        stage('Test') {
            steps {
                bat 'mvn test'
            }
        }

        stage('Build Docker Image') {
            steps {
                bat 'docker build -t ephraimimmanuelh/calculator-app .'
            }
        }

        stage('Push Docker Image') {
            steps {
                withCredentials([usernamePassword(
                    credentialsId: 'dockerhub-creds',
                    usernameVariable: 'DOCKER_USER',
                    passwordVariable: 'DOCKER_PASS'
                )]) {

                    bat 'docker login -u %DOCKER_USER% -p %DOCKER_PASS%'
                    bat 'docker push ephraimimmanuelh/calculator-app'
                }
            }
        }

        stage('Deploy Container') {
            steps {
                bat 'docker stop calculator-container || exit 0'
                bat 'docker rm calculator-container || exit 0'
                bat 'docker run -d -p 8081:80 --name calculator-container ephraimimmanuelh/calculator-app'
            }
        }
    }
}
