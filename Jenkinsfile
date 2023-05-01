pipeline {
    agent any

    stages {
        
        stage('Build') {
            steps {
               
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
                sh 'nohup ./docker-compose up -d &'
            }
        }
    }
}
