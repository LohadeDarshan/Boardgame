pipeline {
    agent any

    tools {
        jdk 'Jdk17'       // Jenkins me configured Java 17 tool
        maven 'Maven3'    // Jenkins me configured Maven tool
    }

    stages {
        stage('SCM Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/LohadeDarshan/Boardgame.git'
            }
        }

        stage('Code Validate') {
            steps {
                withMaven(jdk: 'Jdk17', maven: 'Maven3', traceability: true) {
                    sh 'mvn validate'
                }
            }
        }

        stage('Code Compile') {
            steps {
                withMaven(jdk: 'Jdk17', maven: 'Maven3', traceability: true) {
                    sh 'mvn compile'
                }
            }
        }

        stage('Code Test') {
            steps {
                withMaven(jdk: 'Jdk17', maven: 'Maven3', traceability: true) {
                    sh 'mvn test'
                }
            }
        }
    }
}