
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
                withSonarQubeEnv('SonarQube') {
                    bat 'mvn sonar:sonar -Dsonar.projectKey=YunisBarudi_spring-petclinic -Dsonar.organization=yunisbarudi'
                }
            }
        }
        stage('Package') {
            steps {
                bat 'mvn package -DskipTests'
            }
        }
        stage('Docker Build & Push') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'dockerhub-credentials', usernameVariable: 'DOCKER_USER', passwordVariable: 'DOCKER_PASS')]) {
                    bat "docker build -t yunis111/spring-petclinic:%BUILD_NUMBER% ."
                    bat "docker login -u %DOCKER_USER% -p %DOCKER_PASS%"
                    bat "docker push yunis111/spring-petclinic:%BUILD_NUMBER%"
                    bat "docker tag yunis111/spring-petclinic:%BUILD_NUMBER% yunis111/spring-petclinic:latest"
                    bat "docker push yunis111/spring-petclinic:latest"
                }
            }
        }
        stage('Deploy to EC2 (Ansible)') {
            steps {
                bat 'wsl -d Ubuntu -e bash -c "cd /mnt/c/Users/User/Desktop/cloud/ansible && ansible-playbook -i inventory.ini playbook.yml"'
            }
        }
        stage('Deploy to Kubernetes') {
            steps {
                bat "kubectl apply -f kubernetes\\deployment.yaml -f kubernetes\\service.yaml"
                bat "kubectl set image deployment/spring-petclinic spring-petclinic=yunis111/spring-petclinic:%BUILD_NUMBER%"
                bat "kubectl rollout status deployment/spring-petclinic --timeout=120s"
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