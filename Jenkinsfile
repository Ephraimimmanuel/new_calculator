pipeline {
    agent any

    tools {
        maven 'Maven'
        jdk 'Java17'
    }

    stages {

        stage('Clone') {
            steps {
                git 'https://github.com/Ephraimimmanuel/new_calculator.git'
            }
        }

        stage('Build') {
            steps {
                bat 'mvn clean package'
            }
        }

        stage('Test') {
            steps {
                bat 'mvn test'
            }
        }

        stage('Build Docker Image') {
            steps {
                bat 'docker build -t ephraimimmanuel/calculator-app .'
            }
        }

        stage('Push Docker Image') {
            steps {
                bat 'docker push ephraimimmanuel/calculator-app'
            }
        }

        stage('Deploy Container') {
            steps {
                bat '''
                docker stop calculator || exit 0
                docker rm calculator || exit 0

                docker run -d -p 8081:8080 --name calculator ephraimimmanuel/calculator-app
                '''
            }
        }
    }
}
