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
    }

    stages {
        stage('Build') {
            agent {
                docker { 
                    image 'node:22.11.0-alpine3.20'
                    args '-u root'
                    reuseNode true //reuse the node for the next stage
                    
                }
            }
        

            steps {
                step('clean up workspace') {
                    cleanWs()
                }
                step('Build project'){
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
        }
    }
}