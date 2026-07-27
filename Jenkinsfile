pipeline {
    agent any

    stages {

        stage('Checkout') {
            steps {
                git branch: 'develop',
                    url: 'https://github.com/sahilsalmanov/erp-frontend.git'
            }
        }

        stage('Install Dependencies') {
            steps {
                bat 'npm ci'
            }
        }

        stage('Build') {
            steps {
                bat 'npm run build'
            }
        }

        stage('Docker Build') {
            steps {
                bat 'docker build -t sahillsalmanov/erp-frontend:latest .'
            }
        }

        stage('Docker Push') {
            steps {
                withCredentials([usernamePassword(
                    credentialsId: 'dockerhub',
                    usernameVariable: 'DOCKER_USER',
                    passwordVariable: 'DOCKER_PASS'
                )]) {
                    bat '''
                    @echo off
                    echo|set /p=%DOCKER_PASS%|docker login -u %DOCKER_USER% --password-stdin
                    docker push sahillsalmanov/erp-frontend:latest
                    docker logout
                    '''
                }
            }
        }  
    }
}