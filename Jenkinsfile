pipeline {
    agent any

    tools {
        sonarRunner 'SonarScanner'
    }

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
                    ${SONAR_RUNNER_HOME}/bin/sonar-scanner \
                    -Dsonar.projectKey=static-website \
                    -Dsonar.sources=.
                    '''
                }
            }
        }

        stage('Build Docker Image') {
            steps {
                sh 'docker build -t static-site .'
            }
        }

        stage('Deploy Website') {
            steps {
                sh '''
                docker rm -f static-site || true
                docker run -d -p 80:80 --name static-site static-site
                '''
            }
        }
    }
}
