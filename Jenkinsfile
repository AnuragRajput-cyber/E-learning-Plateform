pipeline {
    agent any  // Use any available Jenkins agent
    
    environment {
        AWS_CREDENTIALS = credentials('aws-access-key') // Single AWS credential stored in Jenkins
        S3_BUCKET = 'mern-frontend-bucket'
        LAMBDA_FUNCTION_NAME = 'mern-backend-function'
    }
    
    stages {
        stage('Clone Repository') {
            steps {
                git branch: 'master', url: 'https://github.com/AnuragRajput-cyber/E-learning-Plateform.git'
            }
        }
        
        stage('Install Dependencies') {
            steps {
                sh 'npm install --prefix server'  // Updated backend folder name to "server"
                sh 'npm install --prefix frontend'
            }
        }

        stage('Build Frontend') {
            steps {
                sh 'npm run build --prefix frontend'
            }
        }
        
        stage('Upload Frontend to S3') {
            steps {
                withAWS(credentials: 'aws-access-key', region: 'us-east-1') {
                    sh 'aws s3 sync frontend/build s3://$S3_BUCKET --delete'
                }
            }
        }

        stage('Build Server') { // If your server requires building, uncomment below
            steps {
                sh 'npm run build --prefix server'  // If your backend has a build step
            }
        }

        stage('Prepare Lambda Package') {
            steps {
                sh '''
                cd server
                zip -r ../server.zip * .env node_modules
                '''
            }
        }

        stage('Deploy Lambda') {
            steps {
                withAWS(credentials: 'aws-access-key', region: 'us-east-1') {
                    sh 'aws lambda update-function-code --function-name $LAMBDA_FUNCTION_NAME --zip-file fileb://server.zip'
                }
            }
        }
    }
}
