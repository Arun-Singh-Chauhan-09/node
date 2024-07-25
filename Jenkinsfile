pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                // Clone the repository
                git branch: 'main', url: 'https://github.com/Arun-Singh-Chauhan-09/node.git', credentialsId: 'github-creds'
            }
        }
        stage('Install Dependencies') {
            steps {
                // Verify Node.js and npm are installed
                sh 'node -v'
                sh 'npm -v'
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
/*        stage('Test') {
            when {
                expression {
                    return fileExists('package.json') && sh(script: 'npm run', returnStdout: true).contains(' test')
                }
            }
            steps {
                // Run tests
                sh 'npm test'
            }
        }  */
        stage('Deploy') {
            steps {
                // Run SSH commands directly
                withCredentials([sshUserPrivateKey(credentialsId: 'node-server-ssh-key', keyFileVariable: 'SSH_KEY')]) {
                    sh '''
                        ssh -i $SSH_KEY -o StrictHostKeyChecking=no ubuntu@172.31.49.97 <<EOF
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

