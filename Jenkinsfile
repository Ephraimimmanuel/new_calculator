pipeline {
agent any
tools {
    maven 'Maven'
    jdk 'Java17'
}

stages {

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
            bat 'docker build -t ephraimimmanuelh/calculator-app .'
        }
    }

   stage('Push Docker Image') {
    steps {
        bat 'docker push ephraimimmanuelh/calculator-app'
    }
}

    stage('Deploy Container') {
    steps {
        bat 'docker stop calculator || exit 0'
        bat 'docker rm calculator || exit 0'
        bat 'docker run -d -p 9090:80 --name calculator ephraimimmanuelh/calculator-app'
    }
}
}
}
