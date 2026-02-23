pipeline {
    agent any
    environment {
        SECOND_SERVER = '16.16.201.91'
        SSH_CREDENTIALS_ID = 'SECOND_SERVER_CREDENTIALS_ID'
        REPO_URL = 'https://github.com/Anar29/juice-shop.git'
        BRANCH = 'master'
    }
    stages {
        stage('Clone Repo') {
            steps {
                echo 'Cloning repo to Jenkins workspace...'
                git branch: "${BRANCH}", url: "${REPO_URL}"
            }
        }
        stage('Install Dependencies') {
            steps {
                echo 'Installing Node.js dependencies...'
                sh 'cd juice-shop && npm install'
            }
        }
        stage('Deploy to Second Server') {
            steps {
                echo 'Deploying Juice Shop to second server...'
                sshagent([SSH_CREDENTIALS_ID]) {
                    sh """
                    ssh -o StrictHostKeyChecking=no ubuntu@${SECOND_SERVER} '
                        if [ ! -d /home/ubuntu/juice-shop ]; then
                            git clone ${REPO_URL} /home/ubuntu/juice-shop
                        else
                            cd /home/ubuntu/juice-shop
                            git reset --hard
                            git pull
                        fi

                        cd /home/ubuntu/juice-shop
                        npm install
                        npm start &
                        echo "Juice Shop deployed successfully!"
                    '
                    """
                }
            }
        }
    }
    post {
        success {
            echo 'Pipeline completed successfully 🚀'
        }
        failure {
            echo 'Pipeline failed ❌'
        }
    }
}
