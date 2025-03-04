pipeline {
    agent any  // Use any available Jenkins agent
    
    environment {
        AWS_CREDENTIALS = credentials('aws-access-key') // Ensure correct AWS credentials ID in Jenkins
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
                sh '''
                # Remove old node_modules & package-lock.json (prevents corruption)
                rm -rf frontend/node_modules frontend/package-lock.json
                rm -rf server/node_modules server/package-lock.json

                # Install dependencies with retry (avoids npm network failures)
                retry(3) {
                    npm install --prefix server
                    npm install --prefix frontend
                }
                '''
            }
        }

        stage('Build Frontend') {
            steps {
                sh '''
                cd frontend
                npm rebuild  # Fix issues with native modules
                npm run build
                '''
            }
        }
        
        stage('Upload Frontend to S3') {
            steps {
                withAWS(credentials: 'aws-access-key', region: "$AWS_REGION") {
                    sh 'aws s3 sync frontend/dist s3://$S3_BUCKET --delete'
                }
            }
        }

        stage('Build Server') { 
            steps {
                sh '''
                cd server
                npm run build || echo "No build step defined, skipping..."
                '''
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
                withAWS(credentials: 'aws-access-key', region: "$AWS_REGION") {
                    sh 'aws lambda update-function-code --function-name $LAMBDA_FUNCTION_NAME --zip-file fileb://server.zip'
                }
            }
        }
    }
}
