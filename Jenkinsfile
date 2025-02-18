pipeline {
    agent any
    
    triggers {
        githubPush()
    }
    
    tools {
        nodejs 'nodejs'  // Ensure this is correctly configured in Jenkins
    }
    
    stages {
        stage('Checkout') {
            steps {
                script {
                    checkout([
                        $class: 'GitSCM',
                        branches: [[name: '*/jenkins']],
                        userRemoteConfigs: [[
                            url: 'https://github.com/soundn/CICD-react-frontend.git',
                            credentialsId: 'new'  // Add Git credentials
                        ]]
                    ])
                }
            }
        }
        
        stage('Setup Node.js') {
            steps {
                sh 'node --version'
                sh 'npm --version'
            }
        }
        
        stage('Clean Workspace') {
            steps {
                sh 'rm -rf node_modules package-lock.json'
                sh 'npm cache clean --force'
            }
        }
        
        stage('Install Dependencies') {
            steps {
                sh 'npm install'
            }
        }
        
        stage('Build Project') {
            steps {
                sh 'npm run build'
            }
        }
        
        stage('Archive Artifact') {
            steps {
                archiveArtifacts artifacts: 'dist/**/*', fingerprint: true
            }
        }
        
        stage('Deploy') {
            steps {
                sshagent(credentials: ['ssh-credential-id']) {
                    sh '''
                        mkdir -p ~/.ssh
                        ssh-keyscan -H 44.204.240.11 >> ~/.ssh/known_hosts
                        scp -r dist/* ubuntu@176.58.103.72:/tmp/react-app
                        
                        ssh -o StrictHostKeyChecking=no ubuntu@176.58.103.72 "
                            sudo apt update && sudo apt upgrade -y
                            echo 'Successful Cache Update'
                            sudo apt install nginx -y
                            echo 'Successful Nginx Installation'
                            sudo systemctl start nginx
                            sudo cp -r /tmp/react-app/* /var/www/html
                            sudo systemctl restart nginx
                        "    
                    '''
                }
            }
        }
    }
    
    post {
        always {
            cleanWs()
        }
    }
}
