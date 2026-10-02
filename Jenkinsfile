pipeline {
    agent any
    stages {
        stage('Build') {
            steps {
                echo 'Building Docker image...'
                sh 'docker build -t jenkins-demo:latest .'
            }
        }

        stage('Test') {
            steps {
                echo 'Testing Docker image...'
                sh 'docker run --rm jenkins-demo:latest nginx -t'
            }
        }

        stage('Deploy') {
            steps {
                echo 'Deploying application...'

                sh '''
                    docker stop jenkins-demo || true
                    docker rm jenkins-demo || true

                    docker run -d \
                        --name jenkins-demo \
                        -p 9000:80 \
                        jenkins-demo:latest
                '''
            }
        }
    }
}