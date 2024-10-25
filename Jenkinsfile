pipeline {
    agent any

    stages {
        stage('Checkout Code') {
            steps {
                // Step 1: Checkout the repository code
                checkout scm
            }
        }

        stage('Set Up Python') {
            steps {
                // Step 2: Set up Python environment and install dependencies
                sh '''
                python3 -m pip install --upgrade pip
                pip3 install -r requirements.txt
                '''
            }
        }

        stage('Run Tests') {
            steps {
                // Step 3: Run pytest to execute tests
                sh 'pytest -v tests/'  // Run tests with verbose output
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
