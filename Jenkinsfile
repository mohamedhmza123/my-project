pipeline {
    agent any
    environment{
        DOCKERHUB_CREDENTIALS = credentials('dockerhub')
         GITHUB_CREDENTIALS = credentials('github')
   }

    stages {
        // Stage 1: Pull the latest code from GitHub
         stage('docker login'){
           steps{
                 
                   
                       
                         sh '''
                         echo docker login -u $DOCKERHUB_CREDENTIALS_USR | $DOCKERHUB_CREDENTIALS_PSW --password-stdin
                         '''
                 
             }
         }
             stage('github login'){
             steps{
                 
                   
                       
                         sh '''
                         echo docker login -u $GITHUB_CREDENTIALS_USR | $GITHUB_CREDENTIALS_PSW --password-stdin
                         '''
                 
             }
        }
        stage('Checkout Code') {
            steps {
                   git branch: 'main',
            url: 'https://github.com/mohamedhmza123/my-project.git'
                checkout scm
            }
        }

        // Stage 2: Test / Validate the Ansible Playbook syntax
        stage('Test Playbook') {
            steps {
                echo 'Running Ansible syntax check...'
                sh 'ansible-playbook -i inventory docker-install.yml --syntax-check'
            }
        }

        // Stage 3: Deploy to Host using Ansible
        stage('Deploy to Host') {
            steps {
                echo 'Executing Ansible deployment playbook...'
                sh 'ansible-playbook -i inventory docker-install.yml'
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
