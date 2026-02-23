pipeline {
    agent any

    environment {
        SECOND_SERVER = '16.16.201.91'  // ikinci AWS serverin public IP-si
    }

    stages {
        stage('Clone Repo') {
            steps {
                echo 'Cloning repo locally on Jenkins...'
                git branch: 'main', url: 'https://github.com/Anar29/juice-shop'
            }
        }

        stage('Deploy to Second Server') {
            steps {
                sshagent(['SECOND_SERVER_CREDENTIALS_ID']) {
                    sh """
                    ssh -o StrictHostKeyChecking=no ubuntu@${SECOND_SERVER} '
                        echo "Deploying Juice Shop..."
                        
                        # App qovluğunu yoxla
                        if [ ! -d /home/ubuntu/juice-shop ]; then
                            git clone https://github.com/Anar29/juice-shop.git /home/ubuntu/juice-shop
                        else
                            cd /home/ubuntu/juice-shop
                            git reset --hard
                            git pull
                        fi

                        # Node.js paketlərini qur
                        cd /home/ubuntu/juice-shop
                        npm install

                        # App-i background-da işə sal
                        npm start &
                        echo "Juice Shop deployed successfully!"
                    '
                    """
                }
            }
        }
    }
}
