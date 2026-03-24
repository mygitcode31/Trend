pipeline {
    agent any

    stages {

        stage('Clone Code') {
            steps {
                checkout scm
            }
        }

        stage('Build Docker Image') {
            steps {
                sh 'docker build -t mydockerimage31/trend-app-image:latest .'
            }
        }

        stage('Login to DockerHub') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'dockerhub-creds', usernameVariable: 'USER', passwordVariable: 'PASS')]) {
                    sh '''
                    echo $PASS | docker login -u $USER --password-stdin
                    '''
                }
            }
        }

        stage('Push to DockerHub') {
            steps {
                sh 'docker push mydockerimage31/trend-app-image:latest'
            }
        }

        stage('Deploy to Kubernetes') {
            steps {
                sh 'kubectl apply -f deployment.yml'
                sh 'kubectl apply -f service.yml'
            }
        }
    }
}
