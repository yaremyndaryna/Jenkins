pipeline {
    agent any

    stages {
        stage('Install Apache2') {
            steps {
                script {
                    sh '''
                        echo "Updating system..."
                        sudo apt update -y

                        echo "Installing Apache2..."
                        sudo apt install apache2 -y

                        echo "Starting Apache2..."
                        sudo systemctl enable apache2
                        sudo systemctl start apache2

                        echo "Apache2 installed and running!"
                    '''
                }
            }
        }

        stage('Check Logs for 4xx/5xx Errors') {
            steps {
                script {
                    sh """
                        LOG_FILE="/var/log/apache2/access.log"
                        if [ -f "$LOG_FILE" ]; then
                            echo "Checking for 4xx and 5xx errors..."
                            grep 'HTTP/1.[01]" [45][0-9][0-9]' "$LOG_FILE" || echo "No errors found."
                        else
                            echo "Apache log file not found!"
                        fi
                    """
                }
            }
        }
    }
}
