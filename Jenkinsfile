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
                    script {
                        def scannerHome = tool 'SonarScanner'
                        sh """
                        ${scannerHome}/bin/sonar-scanner \
                        -Dsonar.projectKey=static-website \
                        -Dsonar.sources=.
                        """
                    }
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
