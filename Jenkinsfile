pipeline {
    agent any
    stages {
        stage('Toolchain') {
            steps {
                sh '''
                    if ! command -v ruby >/dev/null 2>&1 || ! command -v gem >/dev/null 2>&1; then
                        sudo apt-get update && sudo apt-get install -y ruby-full build-essential zlib1g-dev
                    fi
                    if ! command -v bundle >/dev/null 2>&1; then
                        sudo gem install bundler
                    fi
                '''
            }
        }
        stage('Build') {
            steps {
                sh '''
                    bundle config set path 'vendor/bundle'
                    bundle install
                    bundle exec jekyll build
                '''
            }
        }
        stage('Deploy') {
            steps {
                sh '''
                    mkdir -p /var/www/html/jackmanimationtest
                    rsync -a --delete _site/ /var/www/html/jackmanimationtest/
                '''
            }
        }
    }
}
