
pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Check Laravel Project') {
            steps {
                sh 'test -f artisan'
                sh 'echo "Laravel project found successfully"'
            }
        }

        stage('Check PHP and Composer') {
            steps {
                sh 'php --version'
                sh 'composer --version'
            }
        }
    }
}
