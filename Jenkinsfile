pipeline {
    agent any

    environment {
        NODE_VERSION = '18.x'
    }

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Setup Node.js') {
            steps {
                script {
                    if (isUnix()) {
                        sh '''
                            if ! command -v node &> /dev/null; then
                                echo "Node.js not found, installing..."
                                curl -fsSL https://deb.nodesource.com/setup_18.x | sudo -E bash -
                                sudo apt-get install -y nodejs
                            fi
                            node --version
                            npm --version
                        '''
                    } else {
                        bat '''
                            node --version
                            npm --version
                        '''
                    }
                }
            }
        }

        stage('Install dependencies') {
            steps {
                script {
                    if (isUnix()) {
                        sh 'npm install'
                    } else {
                        bat 'npm install'
                    }
                }
            }
        }

        stage('Start app in background') {
            steps {
                script {
                    if (isUnix()) {
                        sh '''
                            nohup npm start > app.log 2>&1 &
                            sleep 5
                        '''
                    } else {
                        bat '''
                            start /b npm start
                            timeout /t 5
                        '''
                    }
                }
            }
        }

        stage('Run tests') {
            steps {
                script {
                    if (isUnix()) {
                        sh 'npm test'
                    } else {
                        bat 'npm test'
                    }
                }
            }
        }
    }

    post {
        always {
            echo 'CI Pipeline completed.'
        }
    }
}

