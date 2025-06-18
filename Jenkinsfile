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

        stage('Deploy') {
            steps {
                docker build -t %DockerUserName%/studentsappdevops:1 .
                docker login -u %DockerUserName% --password %DockerPassword%  
                docker push %DockerUserName%/studentsappdevops:1      
            }
        }

        stage('Manual Approval for Deployment') {
            steps {
                input message: 'Deploy to production?', 
                      ok: 'Deploy',
                      submitterParameter: 'DEPLOYER'
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
