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
                    rsync -rvc \
                      --no-owner \
                      --no-group \
                      --no-perms \
                      --omit-dir-times \
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

        stage('Build Docker Image') {
            steps {
                sh 'docker build -t laravel-app-docker .'
            }
        }

        stage('Deploy Docker Container') {
            steps {
                sh '''
                    docker rm -f laravel-docker || true

                    docker run -d \
                      --name laravel-docker \
                      --restart unless-stopped \
                      -p 8001:80 \
                      --env-file /var/www/laravel-app/.env \
                      -v /var/www/laravel-app/database/database.sqlite:/var/www/html/database/database.sqlite \
                      laravel-app-docker
                '''
            }
        }

        stage('Verify Docker Deployment') {
            steps {
                sh 'sleep 5'
                sh 'curl --fail http://localhost:8001'
                sh 'echo "Docker deployment successful"'
            }
        }
    }
}
