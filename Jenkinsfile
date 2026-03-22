
```groovy
pipeline {
    agent any

    tools {
        maven 'Maven 3.9'
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
            mail to: 'your@email.com', subject: 'Build SUCCESS', body: 'Pipeline passed!'
        }
        failure {
            mail to: 'your@email.com', subject: 'Build FAILED', body: 'Pipeline failed!'
        }
    }
}
```