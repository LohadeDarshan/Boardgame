pipeline {
    agent any
    stages {
        stage('scm checkout') {
            steps {
                git branch: 'master', url: 'https://github.com/LohadeDarshan/Boardgame.git'
            }
        }
        stage('code validate') {
            steps {
                withMaven(globalMavenSettingsConfig: '', jdk: 'Jdk-11', maven: 'MAVEN_HOME', mavenSettingsConfig: '', traceability: true) {
                    sh 'mvn validate'   // validate the code
                }
            }
        }
        stage('code compile') {
            steps {
                withMaven(globalMavenSettingsConfig: '', jdk: 'Jdk-11', maven: 'MAVEN_HOME', mavenSettingsConfig: '', traceability: true) {
                    sh 'mvn compile'   // compile
                }
            }
        }
        stage('code build and scan') {
            steps {
                withMaven(globalMavenSettingsConfig: '', jdk: 'Jdk-11', maven: 'MAVEN_HOME', mavenSettingsConfig: '', traceability: true) {
                    withSonarQubeEnv(credentialsId: 'sonar-token', installationName: 'sonar') {
                        sh 'mvn package sonar:sonar'   // validate + compile + test + package
                    }
                }
            }
        }
        stage('code test') {
            steps {
                withMaven(globalMavenSettingsConfig: '', jdk: 'Jdk-11', maven: 'MAVEN_HOME', mavenSettingsConfig: '', traceability: true) {
                    sh 'mvn test'   // test
                }
            }
        }
        stage('build the code') {
      steps {
        withMaven(globalMavenSettingsConfig: '', jdk: 'Jdk-11', maven: 'MAVEN_HOME', mavenSettingsConfig: '', traceability: true) {
          sh 'mvn clean package'
        }
      }
    }
  }
}