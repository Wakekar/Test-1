pipeline {
    agent any

    stages {

        stage('Prepare Workspace') {
            steps {
                sh '''
                    rm -rf /mnt/project
                    mkdir -p /mnt/project
                '''
            }
        }

        stage('Clone Repository') {
            steps {
                sh '''
                    git clone -b main --depth 1 https://github.com/Wakekar/Test-1.git /mnt/project
                '''
            }
        }

        stage('Deploy to Apache') {
            steps {
                sh '''
                    rm -f /var/www/html/index.html
                    cp /mnt/project/index.html /var/www/html/
                    chmod 644 /var/www/html/index.html

                    if systemctl list-units --type=service | grep -q httpd; then
                        systemctl restart httpd
                    else
                        systemctl restart apache2
                    fi
                '''
            }
        }

    }

    post {
        success {
            echo 'Deployment completed successfully!'
        }
        failure {
            echo 'Deployment failed! Check the console output.'
        }
    }
}
