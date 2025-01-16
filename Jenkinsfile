pipeline {
    agent {
        label 'devops1-ikin'
    }
    
    tools {nodejs "node18"}

    stages {
        stage('Checkout SCM') {
            steps {
                git branch: 'dev-compose', url: 'https://github.com/msodikinn/simple-apps'
            }
        }
        stage('Build') {
            steps {
                sh '''cd apps
                npm install'''
            }
        }
        stage('Testing') {
            steps {
                sh '''cd apps
                npm test
                npm run test:coverage'''
            }
        }
        stage('Code Review') {
            steps {
                sh '''cd apps
                    sonar-scanner \\
                    -Dsonar.projectKey=Simple-Apps \\
                    -Dsonar.sources=. \\
                    -Dsonar.host.url=http://172.23.10.11:9480 \\
                    -Dsonar.login=sqp_aa576e4c1edf73bc20edd53d83779ed7f482bffc'''
            }
        }
        stage('Deploy compose') {
            steps {
                sh '''
                docker compose build
                docker compose up -d
                '''
            }
        }
    }
}