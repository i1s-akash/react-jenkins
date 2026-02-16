pipeline {
    agent any

    tools {
        nodejs 'node-20'
    }

    environment {
        CI = 'false'
    }

    stages {

        stage('Verify Node') {
            steps {
                sh 'node -v'
            }
        }

        stage('Checkout') {
            steps {
                git branch: 'main',
                    url: 'https://github.com/i1s-akash/react-jenkins.git'
            }
        }

        stage('Install Dependencies') {
            steps {
                sh 'npm install'
            }
        }

        stage('Build') {
            steps {
                sh 'npm run build'
            }
        }

        stage('Deploy') {
            steps {
                sh '''
                    echo "Deploying to Nginx folder..."
                    rm -rf /var/www/frontend/*
                    cp -r dist/. /var/www/frontend/
                '''
            }
        }

    }

    post {
        success {
            echo '✅ React build successful'
        }
        failure {
            echo '❌ React build failed'
        }
    }
}
