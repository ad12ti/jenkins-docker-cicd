pipeline {
    agent any

    stages {

        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('SonarQube Analysis') {
            steps {
                withSonarQubeEnv('SonarQube') {
                    sh '''
                        sonar-scanner \
                        -Dsonar.projectKey=jenkins-docker-cicd \
                        -Dsonar.sources=.
                    '''
                }
            }
        }

        stage('Docker Build') {
            steps {
                sh 'docker build -t my-web-app .'
            }
        }

        stage('Deploy') {
            steps {
                sh '''
                    docker stop my-web-app || true
                    docker rm my-web-app || true
                    docker run -d --name my-web-app -p 8081:80 my-web-app
                '''
            }
        }

        stage('Validate Deployment') {
            steps {
                sh '''
                    docker ps
                    docker port my-web-app
                    curl -I http://localhost:8081
                '''
            }
        }
    }
}
