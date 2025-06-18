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
                bat 'docker build -t studentappdevops:1 .'
                bat 'docker stop studentsappdevops || exit 0'
                bat 'docker rm studentsappdevops || exit 0'
                bat 'docker run -d --name studentsappdevops -p 3030:3030 studentsappdevops:1'
                echo 'Docker container deployed'
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
