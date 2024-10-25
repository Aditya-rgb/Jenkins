# Jenkins Pipeline Project

This document outlines the steps taken to set up and configure a Jenkins pipeline on an EC2 instance. It covers installing Jenkins, resolving issues, installing dependencies, setting up virtual environments, running tests, and deploying to a staging environment after successful testing.

## Table of Contents

- [Introduction](#introduction)
- [Jenkins Setup](#jenkins-setup)
- [Environment Setup](#environment-setup)
- [Jenkinsfile Pipeline](#jenkinsfile-pipeline)
  - [Checkout Stage](#checkout-stage)
  - [Install pip](#install-pip)
  - [Install Python venv](#install-python-venv)
  - [Check Python and pip Installation](#check-python-and-pip-installation)
  - [Set Up Python](#set-up-python)
  - [Run Tests](#run-tests)
  - [Move to Staging Environment](#move-to-staging-environment)
- [Issues Resolved](#issues-resolved)
- [Conclusion](#conclusion)

## Introduction

The goal of this project was to set up a Jenkins pipeline that automates the following tasks:
- Checking out the source code
- Installing necessary Python dependencies
- Setting up a virtual environment
- Running tests using pytest
- Moving to the staging environment after successful test completion

This project uses Jenkins on an AWS EC2 instance, a Python virtual environment, and pytest for testing.

## Jenkins Setup

To install Jenkins on the EC2 instance, follow the steps below:

1. **Update and Upgrade Packages:**
    ```bash
    sudo apt update
    sudo apt upgrade -y
    ```

2. **Install Java Development Kit (JDK):**
    Jenkins requires Java, so we install OpenJDK 11:
    ```bash
    sudo apt install openjdk-11-jdk -y
    ```

3. **Add Jenkins Key and Repository:**
    Add Jenkins' GPG key and repository to your system:
    ```bash
    wget -q -O - https://pkg.jenkins.io/debian-stable/jenkins.io.key | sudo tee /usr/share/keyrings/jenkins-keyring.asc
    curl -fsSL https://pkg.jenkins.io/debian-stable/jenkins.io-2023.key | sudo tee /usr/share/keyrings/jenkins-keyring.asc > /dev/null
    ```

4. **Install Jenkins:**
    After adding the repository, update the package list and install Jenkins:
    ```bash
    sudo apt update
    sudo apt install jenkins -y
    ```

5. **Start and Enable Jenkins Service:**
    Start Jenkins and enable it to run on system startup:
    ```bash
    sudo systemctl start jenkins
    sudo systemctl enable jenkins
    ```

6. **Configure Firewall:**
    Allow traffic on Jenkins' default port (8080) and enable OpenSSH:
    ```bash
    sudo ufw allow 8080
    sudo ufw allow OpenSSH
    sudo ufw enable
    ```

7. **Access Jenkins:**
    Retrieve the initial admin password for Jenkins:
    ```bash
    sudo cat /var/lib/jenkins/secrets/initialAdminPassword
    ```

8. **Check Jenkins Status:**
    Verify that Jenkins is running successfully:
    ```bash
    sudo systemctl status jenkins
    ```

9. **Access Jenkins UI:**
    Open your web browser and visit the following URL to complete Jenkins setup:
    ```
    http://<your-ec2-public-ip>:8080
    # http://44.242.147.203:8080/
    ```

    **Note**: Make sure that port `8080` is added as an inbound rule in the AWS EC2 security group.

## Environment Setup

1. **EC2 Setup**: An AWS EC2 instance was used as the environment to host Jenkins.
2. **User Permissions**: The `/etc/sudoers` file was modified to allow the `jenkins` user to run `apt-get` commands without prompting for a password:
    ```bash
    jenkins ALL=(ALL) NOPASSWD: /usr/bin/apt-get
    ```

## Jenkinsfile Pipeline

The Jenkins pipeline was defined in the `Jenkinsfile`. It is responsible for checking out the code, setting up Python, running tests, and moving to staging.

### Checkout Stage

- This stage checks out the source code from the version control system (VCS):
    ```groovy
    stage('Checkout') {
        steps {
            checkout scm
        }
    }
    ```

### Install pip

- In this stage, `pip` is installed using `apt-get`:
    ```groovy
    stage('Install pip') {
        steps {
            sh 'sudo apt-get update && sudo apt-get install -y python3-pip'
        }
    }
    ```

### Install Python venv

- The `python3.12-venv` package is installed to enable virtual environment creation:
    ```groovy
    stage('Install Python venv') {
        steps {
            sh 'sudo apt-get install -y python3.12-venv'
        }
    }
    ```

### Check Python and pip Installation

- This stage checks if Python and pip were installed correctly:
    ```groovy
    stage('Check Python and pip Installation') {
        steps {
            sh '''
                python3 --version
                python3 -m pip --version
            '''
        }
    }
    ```

### Set Up Python

- A virtual environment is created and the required packages from `requirements.txt` are installed:
    ```groovy
    stage('Set Up Python') {
        steps {
            sh '''
                python3 -m venv venv
                . venv/bin/activate
                pip install -r requirements.txt
            '''
        }
    }
    ```

### Run Tests

- The tests are executed using pytest:
    ```groovy
    stage('Run Tests') {
        steps {
            sh '''
                . venv/bin/activate
                pytest
            '''
        }
    }
    ```

### Move to Staging Environment

- After all tests pass, a message indicating that the project is moving to the staging environment is displayed:
    ```groovy
    stage('Move to Staging Environment') {
        steps {
            echo 'Moving to the staging environment as all test cases passed.'
        }
    }
    ```

## Issues Resolved

- **Virtual Environment Setup**: An error related to the absence of `ensurepip` was resolved by installing the `python3.12-venv` package.
- **Sudo Permissions**: The `jenkins` user was granted permissions to run `apt-get` commands without password prompts.

## Conclusion

The Jenkins pipeline was successfully set up on an EC2 instance. It automates the process of checking out the code, installing dependencies, setting up a virtual environment, running tests, and moving to the staging environment if all tests pass.
