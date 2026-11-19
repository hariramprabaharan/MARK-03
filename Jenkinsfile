
pipeline {
    agent any

    stages {
        stage('Install Dependencies') {
            steps {
                sh '''
                    python3 -m venv .venv
                    .venv/bin/pip install -r requirements.txt
                '''
            }
        }

        stage('Run Tests') {
            steps {
                sh '.venv/bin/pytest'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh 'docker build -t mark-03:latest .'
            }
        }

        stage('Push to Docker Hub') {
            steps {
                withCredentials([usernamePassword(
                    credentialsId: 'dockerhub-cred',
                    usernameVariable: 'DOCKER_USERNAME',
                    passwordVariable: 'DOCKER_PASSWORD'
                )]) {
                    sh '''
                        echo "$DOCKER_PASSWORD" | docker login -u "$DOCKER_USERNAME" --password-stdin
                        docker tag mark-03:latest hariramprabaharan23/mark-03:latest
                        docker push hariramprabaharan23/mark-03:latest
                        docker logout
                    '''
                }
            }
        }
    }
}
}
