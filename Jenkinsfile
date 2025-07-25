pipeline{
    agent any
    options {
        skipDefaultCheckout(true) // Skip the default checkout step
    }
    stages{

        stage('Clean up code') {
            steps {
                    cleanWs()
            }
        }

        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Build') {
            agent {
                docker { 
                    image 'node:22.11.0-alpine3.20'
                    args '-u root'
                    reuseNode true //reuse the node for the next stage
                    
                }
            }         

            steps {                
                
                    sh '''
                        ls -l
                        node --version
                        npm --version
                        npm install
                        npm run build
                        ls -l
                    '''               
                
            }
        }

        stage('Test') {
            agent {
                docker { 
                    image 'node:22.11.0-alpine3.20'
                    args '-u root'
                    reuseNode true //reuse the node for the next stage
                }
            }         

            steps {                
                
                    sh '''
                        npm run test
                        test -f dist/index.html
                    '''               
                
            }
        }
        stage('Deploy'){
            agent {
                docker {
                    image 'node:22.11.0-alpine3.20'
                    args '-u root'
                    reuseNode true //reuse the node for the next stage
                }
            }

            steps {
                sh '''
                    npm install -g vercel
                '''
            }
        }
    }
}