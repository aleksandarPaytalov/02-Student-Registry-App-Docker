pipeline {
    agent any
    
    stages {
        stage('Checkout the repository') {
            steps {
                checkout scm
            }
        }
        
        stage('Install Dependencies') {
            steps {
                bat 'npm install'
            }
        }
        
        stage('Run Tests and Audit') {
            parallel {
                stage('Run Tests') {
                    steps {
                        bat 'npm run test'
                    }
                }
                
                stage('Run Audit') {
                    steps {
                        bat 'npm audit'
                    }
                }
            }
        }

        stage('Manual Approval for Deployment') {
            steps {
                input message: 'Deploy to production?', 
                      ok: 'Deploy',
                      submitterParameter: 'DEPLOYER'
            }
        }

         stage('Deploy') {
            steps {
                echo "Deployment approved by: ${DEPLOYER}"
                bat 'taskkill /F /IM node.exe || exit 0'
                bat 'start /B node server.js'
                echo 'Node.js application started'      
            }
        }
    }
    
    post {
        success {
            echo 'Deployment successful!'
        }
        failure {
            echo 'Pipeline failed!'
        }
    }
}
