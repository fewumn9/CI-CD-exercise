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
                            # Load nvm and use Node.js 18.x if installed
                            if command -v nvm >/dev/null 2>&1; then
                                source ~/.nvm/nvm.sh
                                nvm install ${NODE_VERSION}
                                nvm use ${NODE_VERSION}
                            else
                                echo "nvm not found, make sure Node.js ${NODE_VERSION} is installed"
                            fi
                            node -v
                            npm -v
                        '''
                    } else {
                        bat 'node -v && npm -v'
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
                            npm run start &        # Start app in background
                            sleep 5                # Wait for server to boot
                        '''
                    } else {
                        bat '''
                            start /B npm run start
                            timeout /T 5
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

