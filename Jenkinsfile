
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
                bat 'mvn clean compile'
            }
        }
        stage('Test') {
            steps {
                bat 'mvn test'
            }
        }
        stage('Code Quality') {
            steps {
                withSonarQubeEnv('SonarCloud') {
                    bat 'mvn sonar:sonar -Dsonar.projectKey=YunisBarudi_spring-petclinic -Dsonar.organization=yunisbarudi'
                }
            }
        }
        stage('Package') {
            steps {
                bat 'mvn package -DskipTests'
            }
        }
        stage('Deploy') {
            steps {
                bat 'start /B java -jar target\\spring-petclinic-*.jar'
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