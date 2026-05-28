pipeline {
    agent any

    stages {
        stage('1. Install Apache2 on Ubuntu') {
            steps {
                sshagent(['awskey']) {
                    sh '''
                        ssh -o StrictHostKeyChecking=no ubuntu@44.220.89.31 "
                            sudo apt-get update && \
                            sudo apt-get install -y apache2 && \
                            sudo systemctl enable apache2 && \
                            sudo systemctl start apache2
                        "
                    '''
                }
            }
        }
        stage('2. Show OS and check logs for 4xx or 5xx errors') {
            steps {
                sshagent(['awskey']) {
                    sh '''
                        ssh -o StrictHostKeyChecking=no ubuntu@44.220.89.31 "
                            sudo cat /etc/os-release && \
                            sudo grep -E '\" [45][0-9]{2} ' /var/log/apache2/* || echo 'There are no such errors.'
                        "
                    '''
                }
            }
        }
    }
}
