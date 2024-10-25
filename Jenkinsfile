pipeline {
    agent any
    stages {
        stage('Checkout') {
            steps {
                // Replace 'your-credentials-id' with the actual ID of your GitHub credentials in Jenkins
                git credentialsId: 'agent1', url: 'https://github.com/Aditya-rgb/Jenkins', branch: 'main' // or 'master' if that’s your branch name
            }
        }
        stage('Setup Python') {
            steps {
                script {
                    // If you need sudo access, you can add a sudoers entry or modify this step accordingly
                    sh 'sudo apt-get update'
                    sh 'sudo apt-get install -y python3 python3-pip'  // Install required Python packages
                }
            }
        }
        stage('Run Tests') {
            steps {
                sh 'python3 -m unittest discover'  // Run your tests
            }
        }
    }
    post {
        always {
            cleanWs()  // Clean workspace after job completion
        }
    }
}
