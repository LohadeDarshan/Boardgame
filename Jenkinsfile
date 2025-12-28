pipeline {
    agent any

    tools {
        jdk 'Jdk-11'
        maven 'MAVEN_HOME'
    }

    environment {
        SONARQUBE = 'sonar'            // Jenkins me SonarQube server ka name
        SONAR_TOKEN = credentials('sonar-token') // Jenkins me add kiya hua secret text
    }

    stages {
        stage('SCM Checkout') {
            steps {
                git branch: 'master', url: 'https://github.com/LohadeDarshan/Boardgame.git'
            }
        }

        stage('Code Validation') {
            steps {
                withMaven(mavenSettingsConfig: '', traceability: true) {
                    sh 'mvn validate'
                }
            }
        }

        stage('Compile') {
            steps {
                withMaven(mavenSettingsConfig: '', traceability: true) {
                    sh 'mvn compile'
                }
            }
        }

        stage('Test') {
            steps {
                withMaven(mavenSettingsConfig: '', traceability: true) {
                    sh 'mvn test'
                }
            }
        }
        stage('Build & SonarQube Analysis') {
            steps {
                withMaven(mavenSettingsConfig: '', traceability: true) {
                    withSonarQubeEnv(credentialsId: 'sonar-token', installationName: "${env.SONARQUBE}") {
                        sh 'mvn clean package sonar:sonar'
                    }
                }
            }
            }