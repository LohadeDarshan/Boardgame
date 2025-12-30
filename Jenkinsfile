pipeline {
    agent any

    tools {
        jdk 'jdk17'
        maven 'maven3'
    }

    environment {
        SCANNER_HOME = tool 'sonar-scanner'
        SKIP_QG = 'true' // set to 'true' if you want to skip Quality Gate
    }

    stages {
        stage('Git Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/LohadeDarshan/Boardgame.git'
            }
        }

        stage('Compile') {
            steps {
                sh "mvn compile"
            }
        }

        stage('Test') {
            steps {
                sh "mvn test"
            }
        }

        stage('File System Scan') {
            steps {
                sh "trivy fs --format table -o trivy-fs-report.html ."
            }
        }

        stage('SonarQube Analysis') {
            steps {
                withSonarQubeEnv('sonar') {
                    sh """$SCANNER_HOME/bin/sonar-scanner \
                        -Dsonar.projectName=BoardGame \
                        -Dsonar.projectKey=BoardGame \
                        -Dsonar.java.binaries=. """
                }
            }
        }

        stage('Quality Gate') {
            steps {
                script {
                    timeout(time: 2, unit: 'MINUTES') {
                        if (env.SKIP_QG != "true") {
                            waitForQualityGate abortPipeline: false, credentialsId: 'sonar-token'
                        } else {
                            echo "Skipping Quality Gate stage"
                        }
                    }
                }
            }
        }

        stage('Build') {
            steps {
                sh "mvn package"
            }
        }

        stage('Publish To Nexus') {
            steps {
                withMaven(
                    maven: 'maven3', 
                    jdk: 'jdk17',
                    globalMavenSettingsFilePath:: 'e65de56d-41d1-4134-8302-263022f559e4',
                    traceability: true
                ) {
                    sh "mvn deploy"
                }
            }
        }

        stage('Build & Tag Docker Image') {
            steps {
                script {
                    withDockerRegistry(credentialsId: 'myserverd', toolName: 'docker') {
                        sh "docker build -t myserverd/boardshack:latest ."
                    }
                }
            }
        }

        stage('Docker Image Scan') {
            steps {
                sh "trivy image --format table -o trivy-image-report.html myserverd/boardshack:latest"
            }
        }

        stage('Push Docker Image') {
            steps {
                script {
                    withDockerRegistry(credentialsId: 'myserverd', toolName: 'docker') {
                        sh "docker push myserverd/boardshack:latest"
                    }
                }
            }
        }
    } // end of stages
}