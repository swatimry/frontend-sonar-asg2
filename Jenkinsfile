pipeline {
    agent any
    environment {
       
        NODEJS_HOME = 'C:\Program Files\nodejs'
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
                                  -Dsonar.sources=register/src/App.js ^
                                  -Dsonar.host.url=http://localhost:9000 ^
                                  -Dsonar.token=$SONAR_TOKEN
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
