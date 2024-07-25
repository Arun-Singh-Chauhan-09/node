pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                // Clone the repository
                git 'https://github.com/Arun-Singh-Chauhan-09/node.git'
            }
        }
        stage('Install Dependencies') {
            steps {
                // Install dependencies
                sh 'npm install'
            }
        }
        stage('Build') {
            steps {
                // Run build script
                sh 'npm run build'
            }
        }
        stage('Test') {
            steps {
                // Run tests
                sh 'npm test'
            }
        }
        stage('Deploy') {
            steps {
                // SSH into the Node server and restart PM2 service
                sshagent(['node-server-ssh-key']) {
                    sh '''
                        ssh ubuntu@172.31.49.97 <<EOF
                        cd /home/ubuntu/myapp/node
                        git pull origin main
                        npm install
                        pm2 restart all
                        exit
                        EOF
                    '''
                }
            }
        }
    }
}

