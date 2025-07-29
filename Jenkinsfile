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

        stage('Verify Node.js') {
            steps {
                script {
                    if (isUnix()) {
                        sh '''
                            echo "Checking Node.js and npm versions..."
                            node --version || echo "Node.js not found"
                            npm --version || echo "npm not found"
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

        stage('Run tests') {
            steps {
                script {
                    if (isUnix()) {
                        sh 'node ./node_modules/mocha/bin/mocha tests/*.js'
                    } else {
                        bat 'node ./node_modules/mocha/bin/mocha tests/*.js'
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
