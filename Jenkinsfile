pipeline {
    agent {
        docker {
        image 'docker:latest'
        args '-v /var/run/docker.sock:/var/run/docker.sock'
        }
    }

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
                sh 'start ./docker-compose up -d'
            }
        }
    }
}
