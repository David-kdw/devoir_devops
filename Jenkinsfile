pipeline {
    agent any

    environment {
        DOCKER_HOST = "tcp://localhost:2375"
    }

    stages {
        stage('Verify tooling') {
            steps {
                sh '''
                    docker info
                    docker version 
                    docker compose version 
                    jq --version 
                '''
            }
        }
        stage('Build') {
            steps {
                sh 'chmod +x docker-compose'
                sh './docker-compose build'
            }
        }

        stage('Test') {
            steps {
                sh './docker-compose run --rm myapp python -m pytest tests'
            }
        }

        stage('SonarQube analysis') {
            steps {
                withSonarQubeEnv('SonarQube') {
                    sh './docker-compose run --rm myapp sonar-scanner'
                }
            }
        }

        stage('Deploy') {
            steps {
                sh './docker-compose up -d'
            }
        }
    }
}