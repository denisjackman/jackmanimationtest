pipeline {
    agent any
    stages {
        stage('Toolchain check') {
            steps {
                sh '''
                    for tool in ruby gem bundle; do
                        if ! command -v "$tool" >/dev/null 2>&1; then
                            echo "Missing required tool: $tool. Install ruby-full and bundler on this host (jenkins has no passwordless sudo to do it automatically)."
                            exit 1
                        fi
                    done
                '''
            }
        }
        stage('Build') {
            steps {
                sh '''
                    bundle install --path vendor/bundle
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
