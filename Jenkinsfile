pipeline {
    agent any

    triggers {
        // Trigger on SCM changes (e.g., Git)
        pollSCM('* * * * *') // Checks for changes every minute
    }

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }
        stage('Install pip') {
            steps {
                sh 'sudo apt-get update && sudo apt-get install -y python3-pip'
            }
        }
        stage('Install Python venv') {
            steps {
                sh 'sudo apt-get install -y python3.12-venv'
            }
        }
        stage('Check Python and pip Installation') {
            steps {
                sh '''
                    python3 --version
                    python3 -m pip --version
                '''
            }
        }
        stage('Set Up Python') {
            steps {
                sh '''
                    python3 -m venv venv
                    . venv/bin/activate
                    pip install -r requirements.txt
                '''
            }
        }
        stage('Run Tests') {
            steps {
                sh '''
                    . venv/bin/activate
                    pytest
                '''
            }
        }
        stage('Move to Staging Environment') {
            when {
                expression {
                    currentBuild.result == 'SUCCESS'
                }
            }
            steps {
                script {
                    echo 'Moving to staging environment as all test cases passed!'
                }
            }
        }
    }
    post {
        always {
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
