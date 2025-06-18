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
                // Add your deployment commands here
                bat 'npm run deploy'
                // bat 'docker build -t myapp .'
                // bat 'kubectl apply -f deployment.yaml'
                echo 'Deploying application...'
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
