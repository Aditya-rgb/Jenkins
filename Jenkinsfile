pipeline {
    agent any

    stages {
        stage('Checkout Code') {
            steps {
                checkout scm
            }
        }

        stage('Check Python and pip Installation') {
            steps {
                script {
                    // Check if Python is installed
                    def pythonInstalled = sh(script: 'python3 --version', returnStatus: true) == 0
                    // Check if pip is installed
                    def pipInstalled = sh(script: 'python3 -m pip --version', returnStatus: true) == 0

                    if (!pythonInstalled) {
                        error "Python is not installed!"
                    }

                    if (!pipInstalled) {
                        echo "pip is not installed. Installing pip..."
                        // Use curl to download get-pip.py and install pip
                        sh '''
                        curl https://bootstrap.pypa.io/get-pip.py -o get-pip.py
                        python3 get-pip.py
                        '''
                    }
                }
            }
        }

        stage('Set Up Python') {
            steps {
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
