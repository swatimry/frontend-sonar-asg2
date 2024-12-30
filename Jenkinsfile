pipeline {
    agent any
    
    tools {
        nodejs 'nodejs' // The name of the Node.js installation from Global Tool Configuration
    }

    environment {
        SONAR_TOKEN = credentials('sonar-token') // Replace with your SonarQube token Jenkins credential ID
    }

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Install Dependencies') {
            steps {
                script {
                    echo "Installing dependencies..."
                    sh 'npm install'
                }
            }
        }

        stage('Run Tests') {
            steps {
                script {
                    echo "Running tests with coverage..."
                    sh 'npm test -- --coverage' // Adjust based on your test setup
                }
            }
        }

        stage('Build Project') {
            steps {
                script {
                    echo "Building the project..."
                    sh 'npm run build' // Adjust based on your build setup
                }
            }
        }

        stage('SonarQube Analysis') {
            steps {
                script {
                    echo "Running SonarQube analysis..."
                    sh '''
                    sonar-scanner -Dsonar.projectKey=frontend-sonar-asg2 \
                                  -Dsonar.projectName=frontend-sonar-asg2 \
                                  -Dsonar.sources=. \
                                  -Dsonar.javascript.lcov.reportPaths=coverage/lcov-report/index.html \
                                  -Dsonar.host.url=http://localhost:9000 \
                                  -Dsonar.token=$SONAR_TOKEN
                    '''
                }
            }
        }
    }

    post {
        success {
            echo 'Pipeline completed successfully!'
        }
        failure {
            echo 'Pipeline failed. Please check the logs for details.'
        }
        always {
            echo 'This stage always runs, regardless of the result.'
        }
    }
}
