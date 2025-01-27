# E-Learning Platform

## Overview
The **E-Learning Platform** is a robust, cloud-based application built using the MERN stack (MongoDB, Express.js, React, Node.js) and Vite for rapid front-end development. The platform offers users seamless access to online courses, video lectures, and assessments. The app is deployed on AWS using EC2 instances for hosting and S3 for media storage.

---

## Features
- **User Authentication**: Secure login and registration with JWT.
- **Course Management**: Add, update, and delete courses.
- **Video Streaming**: Efficient video delivery using AWS S3.
- **Responsive Design**: Optimized for desktop and mobile devices.
- **Dynamic Search**: Real-time filtering and course searches.
- **Admin Dashboard**: Manage users, courses, and analytics.

---

## Tech Stack
### Front-End
- **React**: Front-end library for building dynamic user interfaces.
- **Vite**: Lightning-fast development environment.

### Back-End
- **Node.js**: JavaScript runtime for server-side logic.
- **Express.js**: Framework for creating RESTful APIs.

### Database
- **MongoDB**: NoSQL database for managing application data.

### Deployment
- **AWS EC2**: Hosts the application back-end and front-end.
- **AWS S3**: Stores static assets and video content.

---

## Architecture
1. **Frontend**: Built with React and Vite for a fast and interactive user experience.
2. **Backend**: Node.js and Express.js handle API requests, authentication, and database operations.
3. **Database**: MongoDB manages data related to users, courses, and progress.
4. **AWS Services**: 
   - EC2 for hosting the application.
   - S3 for scalable storage of course materials and videos.

---

## Installation
### Prerequisites
- Node.js (v14 or later)
- MongoDB (local or cloud-based, e.g., MongoDB Atlas)
- AWS CLI configured with appropriate IAM permissions

### Steps
1. **Clone the repository:**
   ```bash
   git clone <repository-url>
   cd e-learning-platform
   ```

2. **Install dependencies:**
   ```bash
   # Backend
   cd server
   npm install

   # Frontend
   cd ../client
   npm install
   ```

3. **Configure environment variables:**
   Create `.env` files in the `server` and `client` directories:
   - **Server/.env:**
     ```env
     PORT=5000
     MONGO_URI=<your-mongodb-uri>
     JWT_SECRET=<your-secret-key>
     AWS_ACCESS_KEY_ID=<your-aws-access-key>
     AWS_SECRET_ACCESS_KEY=<your-aws-secret-key>
     S3_BUCKET_NAME=<your-s3-bucket-name>
     ```
   - **Client/.env:**
     ```env
     VITE_API_URL=<backend-api-url>
     ```

4. **Start the application:**
   ```bash
   # Backend
   cd server
   npm run start

   # Frontend
   cd ../client
   npm run dev
   ```

---

## Deployment
### Steps to Deploy on AWS
1. **Create EC2 Instances:**
   - Launch separate instances for the backend and frontend.
   - Configure security groups for appropriate ports (e.g., 80, 443, 5000).

2. **Deploy Backend:**
   - SSH into the EC2 instance.
   - Pull the repository and install dependencies.
   - Configure environment variables.
   - Use `pm2` to run the server:
     ```bash
     pm2 start server/index.js --name e-learning-backend
     ```

3. **Deploy Frontend:**
   - Build the React app:
     ```bash
     cd client
     npm run build
     ```
   - Serve the static files using Nginx or Apache.

4. **Setup S3 Bucket:**
   - Upload course videos and static assets to the S3 bucket.
   - Enable public access or configure signed URLs for secure access.

5. **Domain Setup:**
   - Link a custom domain using Route 53.
   - Configure SSL using AWS Certificate Manager.

---

## Usage
1. **User Registration/Login:**
   - Sign up to access the platform.
   - Enroll in available courses.

2. **Course Interaction:**
   - Watch video lectures and complete assessments.
   - Track progress through the user dashboard.

3. **Admin Panel:**
   - Manage users, courses, and upload new materials.

---

## Contributing
Contributions are welcome! Please follow these steps:
1. Fork the repository.
2. Create a feature branch:
   ```bash
   git checkout -b feature/your-feature
   ```
3. Commit changes and push to your fork.
4. Submit a pull request.

---

## License
This project is licensed under the MIT License. See the LICENSE file for details.
