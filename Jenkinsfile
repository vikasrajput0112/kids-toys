pipeline {

    agent any

    stages {

        stage('Checkout Code') {
            steps {
                checkout scm
            }
        }

        stage('Docker Build') {
            steps {
                sh '''
                    docker build -t kids-toys:${BUILD_NUMBER} .
                '''
            }
        }

        stage('Stop Existing Container') {
            steps {
                sh '''
                    docker rm -f kids-toys || true
                '''
            }
        }

        stage('Run Docker Container') {
            steps {
                sh '''
                    docker run -d \
                        --name kids-toys \
                        -p 8081:80 \
                        kids-toys:${BUILD_NUMBER}
                '''
            }
        }

        stage('Verify Website') {
            steps {
                sh '''
                    sleep 5
                    curl -I http://localhost:8081
                '''
            }
        }
    }

    post {
        success {
            echo 'Kids Toys website deployed successfully!'
        }

        failure {
            echo 'Kids Toys deployment failed!'
        }
    }
}
