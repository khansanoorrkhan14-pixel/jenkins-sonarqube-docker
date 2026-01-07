pipeline {
    agent any

    stages {

        stage('Checkout Code') {
            steps {
                checkout scm
            }
        }

        stage('SonarQube Analysis') {
            steps {
                withSonarQubeEnv('SonarQube') {
                    sh '''
                    sonar-scanner \
                      -Dsonar.projectKey=static-website \
                      -Dsonar.sources=.
                    '''
                }
            }
        }

        stage('Build Docker Image') {
            steps {
                sh 'docker build -t static-website .'
            }
        }

        stage('Deploy Website') {
            steps {
                sh '''
                docker rm -f static-website || true
                docker run -d -p 80:80 --name static-website static-website
                '''
            }
        }
    }
}
