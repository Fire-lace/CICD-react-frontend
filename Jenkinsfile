pipeline {
    agent any

    tools {
        nodejs "nodejs"
    }

    stages {
        stage('Clone Repository') {
            steps {
                git 'https://github.com/Fire-lace/CICD-react-frontend.git'
            }
        }

        stage('Install Dependencies') {
            steps {
                sh 'npm install'
            }
        }

        stage('Build React App') {
            steps {
                sh 'npm run build'
            }
        }

        stage('Deploy') {
            steps {
                echo 'Deployment step will be configured later...'
            }
        }
    }
}
