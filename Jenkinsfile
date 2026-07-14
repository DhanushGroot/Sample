pipeline {
    agent any

    tools {
        jdk 'JDK 21'
        maven 'Maven'
    }

    environment {
        SONARQUBE_ENV = 'Sonar'
    }

    stages {

        stage('Checkout') {
            steps {
                git branch: 'main',
                    credentialsId: '44f2d6db-f7e5-489b-a1b5-f5c9ece55da4',
                    url: 'https://github.com/DhanushGroot/Sample.git'
            }
        }

        stage('Build & Test') {
            steps {
                sh 'mvn clean verify'
            }
        }

        stage('SonarQube Analysis') {
            steps {
                withSonarQubeEnv("${SONARQUBE_ENV}") {
                    sh '''
                        mvn sonar:sonar \
                        -Dsonar.projectKey=demo \
                        -Dsonar.projectName=demo
                    '''
                }
            }
        }

        stage('Package') {
            steps {
                sh 'mvn package -DskipTests'
            }
        }

        stage('Deploy to Nexus') {
            steps {
                sh 'mvn deploy -DskipTests'
            }
        }

        stage('Archive Artifact') {
            steps {
                archiveArtifacts artifacts: 'target/*.jar', fingerprint: true
            }
        }
    }

    post {
        success {
            echo 'CI Pipeline completed successfully.'
        }

        failure {
            echo 'Pipeline failed.'
        }

        always {
            cleanWs()
        }
    }
}
