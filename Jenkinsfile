pipeline {
    agent any

    tools {
        jdk 'JAVA_HOME'
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
        stage('code build and scan') {
            steps {
                withMaven(globalMavenSettingsConfig: '', jdk: 'JAVA_HOME', maven: 'MAVEN_HOME', mavenSettingsConfig: '', traceability: true) {
                    withSonarQubeEnv(credentialsId: 'sonar-token', installationName: 'sonar') {
                        sh 'mvn clean package org.sonarsource.scanner.maven:sonar-maven-plugin:sonar'// validate + compile + test + package
                    }
                }
            }
        }
    }
}