pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                // Checkout the source code from the repository
                checkout scm
            }
        }
        stage('Install pip') {
            steps {
                // Update package lists and install pip
                sh 'sudo apt-get update && sudo apt-get install -y python3-pip'
            }
        }
        stage('Check Python and pip Installation') {
            steps {
                // Check the installed versions of Python and pip
                sh '''
                    python3 --version
                    python3 -m pip --version
                '''
            }
        }
        stage('Set Up Python') {
            steps {
                // Create a virtual environment and install requirements
                sh '''
                    python3 -m venv venv
                    source venv/bin/activate
                    pip install -r requirements.txt
                '''
            }
        }
        stage('Run Tests') {
            steps {
                // Run your tests using pytest
                sh '''
                    source venv/bin/activate
                    pytest
                '''
            }
        }
    }
    post {
        always {
            // Clean up the workspace after the build
            cleanWs()
        }
        success {
            echo 'Pipeline completed successfully!'
        }
        failure {
            echo 'Tests failed!'
        }
    }
}
