
pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Validate Laravel') {
            steps {
                sh 'test -f artisan'
                sh 'php --version'
                sh 'composer --version'
                sh 'echo "Laravel project validation successful"'
            }
        }

        
        stage('Deploy to Server') {
            steps {
                sh '''
                    rsync -av --no-owner --no-group \
                      --exclude='.git/' \
                      --exclude='.env' \
                      --exclude='.env.*' \
                      --exclude='database/database.sqlite' \
                      --exclude='storage/' \
                      --exclude='vendor/' \
                      --exclude='bootstrap/cache/' \
                      --exclude='node_modules/' \
                      ./ /var/www/laravel-app/
                '''
            }
        }

    }
}
