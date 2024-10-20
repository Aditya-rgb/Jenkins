pipeline {
    agent any

    stages {
        stage('Setup Python') {
            steps {
                // Specify the Python version you want to use
                pyenv('3.8.6') {
                    // Here you can create a virtual environment and install dependencies
                    sh '''
                    python -m venv venv
                    source venv/bin/activate
                    pip install -r requirements.txt
                    '''
                }
            }
        }
        stage('Run Tests') {
            steps {
                // Execute your tests within the virtual environment
                pyenv('3.8.6') {
                    sh '''
                    source venv/bin/activate
                    pytest
                    '''
                }
            }
        }
    }
}
