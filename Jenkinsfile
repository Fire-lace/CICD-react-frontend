pipeline {
    agent any

    tools {
        nodejs "nodejs" // Ensure the correct Node.js tool is set up in Jenkins
    }

    stages {
        stage('Checkout') {
            steps {
                script {
                    // Checkout from GitHub using credentials
                    checkout([
                        $class: 'GitSCM',
                        branches: [[name: '*/main']], // Replace 'main' with your branch if needed
                        userRemoteConfigs: [[
                            url: 'https://github.com/Fire-lace/CICD-react-frontend.git',
                            credentialsId: 'a66807d7-1713-4c32-87a4-998dc556be20' // Replace with your credentials ID
                        ]]
                    ])
                }
            }
        }

        stage('Install Dependencies') {
            steps {
                sh 'npm install' // Install Node.js dependencies
            }
        }

        stage('Build React App') {
            steps {
                sh 'npm run build' // Build the React app
            }
        }

        stage('Deploy') {
            steps {
                // Add your deployment logic here
                echo 'Deployment step will be configured later...'
            }
        }
    }

    post {
        success {
            echo 'Pipeline executed successfully!'
        }
        failure {
            echo 'Pipeline failed. Please check the logs for errors.'
        }
    }
}
