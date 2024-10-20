pipeline {
    agent any

    stages {
        stage('Checkout SCM') {
            steps {
                checkout scm
            }
        }

        stage('Setup Python') {
            steps {
                script {
                    // Update the package index
                    sh 'sudo apt-get update'
                    // Install Python and pip
                    sh 'sudo apt-get install -y python3 python3-pip'
                    // Install any required packages from requirements.txt
                    sh 'pip3 install -r requirements.txt'
                }
            }
        }

        stage('Run Tests') {
            steps {
                // Run your tests using pytest
                sh 'pytest'
            }
        }
    }

    post {
        always {
            // Clean up workspace after the build
            cleanWs()
        }
    }
}
