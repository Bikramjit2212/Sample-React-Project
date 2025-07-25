pipeline{
    agent any

    //this denotes that this pipeline will only run on test environment
    environment {
        NODE_ENV = 'test'
        VERCEL_TOKEN = credentials('VERCEL_TOKEN') // Use the credentials plugin to securely access the Vercel token
    }
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
                    echo $MY_VAR
                    vercel --prod --token=$VERCEL_TOKEN --confirm --name=cicdproject
                '''
            }
        }
    }
}