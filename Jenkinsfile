pipeline {
    agent any

    stages {
        stage('Checkout Code') {
            steps {
                // Step 1: Checkout the repository code
                checkout scm
            }
        }

        stage('Check Python Version') {
            steps {
                // Verify Python and pip installation
                sh '''
                python3 --version
                pip3 --version
                '''
            }
        }

        stage('Set Up Python') {
            steps {
                // Step 2: Set up Python environment and install dependencies
                sh '''
                set -e  # Fail fast on error
                python3 -m pip install --upgrade pip  # Upgrade pip
                python3 -m venv venv  # Create a virtual environment
                source venv/bin/activate  # Activate the virtual environment
                pip install -r requirements.txt  # Install dependencies
                '''
            }
        }

        stage('Run Tests') {
            steps {
                // Step 3: Run pytest to execute tests
                sh '''
                source venv/bin/activate  # Activate the virtual environment
                pytest -v tests/  # Run tests with verbose output
                '''
            }
        }
    }

    post {
        success {
            echo 'Tests ran successfully!'
        }
        failure {
            echo 'Tests failed!'
        }
    }
}
