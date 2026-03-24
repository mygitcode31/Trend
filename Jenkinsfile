pipeline {
    agent any

    stages {

        stage('Clone Code') {
            steps {
                git branch: 'main',
                    url: 'https://github.com/mygitcode31/Trend.git',
                    credentialId: 'Git-Cred'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh 'docker build -t mydockerimage31/trend-app-image:latest .'
            }
        }

        stage('Login to DockerHub') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'Dockeruser', usernameVariable: 'USER', passwordVariable: 'PASS')]) {
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
