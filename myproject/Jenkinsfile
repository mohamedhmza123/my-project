
pipeline {
    agent any

    environment {
        DOCKERHUB_CREDENTIALS = credentials('dockerhub')
        GITHUB_CREDENTIALS = credentials('github')
    }

    stages {
        stage('Docker Hub Login') {
            steps {
                sh '''
                    set -eu
                    printf '%s' "$DOCKERHUB_CREDENTIALS_PSW" |
                        docker login -u "$DOCKERHUB_CREDENTIALS_USR" --password-stdin
                '''
            }
        }

        stage('GitHub Container Registry Login') {
            steps {
                sh '''
                    set -eu
                    printf '%s' "$GITHUB_CREDENTIALS_PSW" |
                        docker login ghcr.io -u "$GITHUB_CREDENTIALS_USR" --password-stdin
                '''
            }
        }

        stage('Checkout Code') {
            steps {
                checkout scm
            }
        }

        stage('Test Playbook') {
            steps {
                echo 'Running Ansible syntax check...'
                sh 'ansible-playbook -i inventory ansible-playbook.yml --syntax-check'
            }
        }

        stage('Deploy to Host') {
            steps {
                echo 'Executing Ansible deployment playbook...'
                sh 'ansible-playbook -i inventory ansible-playbook.yml'
            }
        }
    }

    post {
        success {
            echo 'Pipeline completed successfully! The container has been built and deployed.'
        }

        failure {
            echo 'Pipeline failed. Please check the logs for errors.'
        }
    }
}
