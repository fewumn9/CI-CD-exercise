pipeline {
    agent any

    environment {
        NODE_ENV = 'development'
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
                            echo "Node.js version:"
                            node -v
                            echo "npm version:"
                            npm -v
                        '''
                    } else {
                        bat '''
                            node -v
                            npm -v
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
                        sh 'npx mocha tests/*.js'
                    } else {
                        bat 'npx mocha tests/*.js'
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
