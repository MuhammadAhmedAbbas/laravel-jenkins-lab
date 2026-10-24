
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
                    rsync -av \
                      --exclude='.env' \
                      --exclude='database/database.sqlite' \
                      --exclude='storage/' \
                      --exclude='vendor/' \
                      ./ /var/www/laravel-app/

                    cd /var/www/laravel-app
                    composer install --no-interaction --prefer-dist --optimize-autoloader
                    php artisan config:clear
                    php artisan route:clear
                    php artisan view:clear
                '''
            }
        }
    }
}
