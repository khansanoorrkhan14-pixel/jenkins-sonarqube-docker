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
                sh 'docker build -t enterprise-site .'
            }
        }

        stage('Deploy Website') {
            steps {
                sh '''
                docker rm -f enterprise-site || true
                docker run -d -p 80:80 --name enterprise-site enterprise-site
                '''
            }
        }
    }
}
