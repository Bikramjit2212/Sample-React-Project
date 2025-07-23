pipeline{
    agent any
    stages {
        stage('Build'){
            agent{
                image 'node:18-alpine'
                reusable true //reuse the node for the next stage
            }
        }

        steps{
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