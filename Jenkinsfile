pipeline {
    agent any

    stages {
        stage('Clone Repo') {
            steps {
                git branch: 'main', url: 'https://github.com/Anar29/juice-shop.git'
            }
        }

        stage('Echo Test') {
            steps {
                echo 'Repo başarıyla Jenkins-ə əlavə edildi!'
            }
        }
    }
}
