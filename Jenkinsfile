
pipeline {
    agent any

    tools {
        maven 'Maven'
    }

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }
        stage('Build') {
            steps {
                sh 'mvn clean compile'
            }
        }
        stage('Test') {
            steps {
                sh 'mvn test'
            }
        }
        stage('Code Quality') {
            steps {
                withSonarQubeEnv('SonarCloud') {
                    sh 'mvn sonar:sonar'
                }
            }
        }
        stage('Package') {
            steps {
                sh 'mvn package -DskipTests'
            }
        }
        stage('Deploy') {
            steps {
                sh 'java -jar target/*.jar &'
            }
        }
    }

    post {
        success {
            mail to: 'unis1barudi@gmail.com', subject: 'Build SUCCESS', body: 'Pipeline passed!'
        }
        failure {
            mail to: 'unis1barudi@gmail.com', subject: 'Build FAILED', body: 'Pipeline failed!'
        }
    }
}