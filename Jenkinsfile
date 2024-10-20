pipeline {
    agent any

    stages {
        // Build stage: Set up the Python environment and install dependencies
        stage('Build') {
            steps {
                script {
                    // Specify the Python version you want to use
                    pyenv '3.9.6' // Adjust to the desired version
                }
                echo 'Installing dependencies...'
                sh 'pip install -r requirements.txt'
            }
        }

        // Test stage: Run unit tests using pytest
        stage('Test') {
            steps {
                echo 'Running unit tests...'
                sh 'pytest tests/test_app.py' // Adjust if you have multiple test files
            }
        }
    }

    post {
        always {
            echo 'Pipeline complete. Cleaning up...'
            cleanWs()
        }
        success {
            echo 'Build and tests were successful!'
        }
        failure {
            echo 'Build or tests failed!'
        }
    }
}
