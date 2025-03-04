pipeline {
    agent any  // Use any available Jenkins agent
    
    environment {
        AWS_CREDENTIALS = credentials('aws-access-key') // Use your correct AWS credential ID
        S3_BUCKET = 'mern-frontend-bucket'
        LAMBDA_FUNCTION_NAME = 'mern-backend-function'
        AWS_REGION = 'us-east-1'
    }
    
    stages {
        stage('Clone Repository') {
            steps {
                git branch: 'master', url: 'https://github.com/AnuragRajput-cyber/E-learning-Plateform.git'
            }
        }
        
        stage('Install Dependencies') {
            steps {
                sh 'rm -rf frontend/node_modules frontend/package-lock.json'
                sh 'rm -rf server/node_modules server/package-lock.json'

                // Retry mechanism added
                retry(3) {
                    sh 'npm install --prefix server'
                    sh 'npm install --prefix frontend'
                }
            }
        }

        stage('Build Frontend') {
            steps {
                sh 'cd frontend && npm rebuild'
                sh 'cd frontend && npm run build'
            }
        }
        
        stage('Upload Frontend to S3') {
            steps {
                withAWS(credentials: 'aws-access-key', region: "$AWS_REGION") { // Updated credential ID
                    sh 'aws s3 sync frontend/dist s3://$S3_BUCKET --delete'
                }
            }
        }

        stage('Build Server') { 
            steps {
                sh 'cd server && npm run build || echo "No build step defined, skipping..."'
            }
        }

        stage('Prepare Lambda Package') {
            steps {
                sh 'cd server && zip -r ../server.zip * .env node_modules'
            }
        }

        stage('Deploy Lambda') {
            steps {
                withAWS(credentials: 'aws-access-key', region: "$AWS_REGION") { // Updated credential ID
                    sh 'aws lambda update-function-code --function-name $LAMBDA_FUNCTION_NAME --zip-file fileb://server.zip'
                }
            }
        }
    }
}
