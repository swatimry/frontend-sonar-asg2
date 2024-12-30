pipeline {
    agent any
    environment {
        NODEJS_HOME = 'C:\\Program Files\\nodejs'  // Corrected path format
    }
    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Install Dependencies') {
            steps {
                bat '''
                echo "Installing dependencies..."
                npm install
                '''
            }
        }

        stage('Build') {
            steps {
                bat '''
                echo "Building the application..."
                npm run build
                '''
            }
        }

        stage('SonarQube Analysis') {
            environment {
                SONAR_TOKEN = credentials('sonar-token')  // Accessing the SonarQube token stored in Jenkins credentials
            }
            steps {
                echo "Running SonarQube analysis..."
                bat '''
                sonar-scanner -Dsonar.projectKey=frontend-sonar-asg2 ^
                              -Dsonar.projectName=frontend-sonar-asg2 ^
                              -Dsonar.sources=register/src ^  // Pointing to the 'src' folder instead of a single file
                              -Dsonar.host.url=http://localhost:9000 ^
                              -Dsonar.token=%SONAR_TOKEN%  // Using %SONAR_TOKEN% for Windows batch
                '''
            }
        }
    }
    post {
        success {
            echo 'Pipeline completed successfully'
        }
        failure {
            echo 'Pipeline failed'
        }
        always {
            echo 'This runs regardless of the result.'
        }
    }
}
