pipeline {
    agent any

    tools {
        maven 'MAVEN_HOME'
    }

    environment {
        SONARQUBE = 'sonar'
    }

    stages {

        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/LohadeDarshan/Boardgame.git'
            }
        }

        stage('Build & Test') {
            steps {
                sh 'mvn clean verify'
            }
        }

        stage('SonarQube Analysis') {
            steps {
                withSonarQubeEnv(
                    installationName: 'sonar',
                    credentialsId: 'sonar-token'
                ) {
                    sh '''
                      mvn sonar:sonar \
                      -Dsonar.projectKey=boardgame \
                      -Dsonar.projectName=Boardgame
                    '''
                }
            }
        }
    }
}
