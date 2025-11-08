# ReviewReady - Local Business Review Monitor Setup Guide

This guide will walk you through setting up the ReviewReady project for local development. Follow each step carefully to ensure proper setup.

## Prerequisites

Before starting, make sure you have the following installed:

1. **Node.js and npm**
   - Install Node.js version 16 or higher
   - You can download it from: https://nodejs.org/
   - Verify installation:
     ```bash
     node --version
     npm --version
     ```

2. **MongoDB**
   - Option 1: Local Installation
     - Download and install MongoDB Community Server from: https://www.mongodb.com/try/download/community
     - Start MongoDB service
   - Option 2: MongoDB Atlas (Cloud)
     - Create a free account at: https://www.mongodb.com/cloud/atlas
     - Create a new cluster
     - Get your connection string

3. **Git** (for version control)
   - Download and install from: https://git-scm.com/downloads

## Step 1: Project Setup

1. **Clone the repository**
   ```bash
   git clone <repository-url>
   cd local-business-review-monitor
   ```

2. **Install global dependencies**
   ```bash
   npm install -g vercel
   ```

3. **Install project dependencies**
   ```bash
   # Install main project dependencies
   npm install

   # Install client dependencies
   cd client
   npm install
   cd ..
   ```

## Step 2: Environment Configuration

1. **Create environment file**
   - Copy `.env.example` to `.env`:
     ```bash
     cp .env.example .env
     ```

2. **Configure environment variables**
   Open `.env` and set the following variables:

   ```bash
   # Server Configuration
   NODE_ENV=development
   PORT=3000

   # Database
   MONGODB_URI=mongodb://localhost:27017/reviewready  # For local MongoDB
   # Or for MongoDB Atlas:
   # MONGODB_URI=mongodb+srv://<username>:<password>@<cluster>.mongodb.net/reviewready

   # Generate a 32-character encryption key (exactly 32 chars)
   ENCRYPTION_KEY=your_32_character_encryption_key_here

   # Admin token for setup endpoints
   ADMIN_TOKEN=your_admin_token_here

   # JWT Configuration (for authentication)
   JWT_SECRET=your_32_character_jwt_secret_here
   JWT_EXPIRES_IN=7d

   # Email Configuration (using Resend)
   RESEND_API_KEY=your_resend_api_key_here
   EMAIL_FROM=noreply@yourdomain.com
   EMAIL_REPLY_TO=support@yourdomain.com

   # Frontend URL
   FRONTEND_URL=http://localhost:3000
   ```

## Step 3: MongoDB Setup

1. **Local MongoDB Setup**
   - After installing MongoDB, create a data directory:
     ```bash
     mkdir -p /data/db  # On Unix/Linux
     # OR
     md C:\data\db     # On Windows
     ```
   - Start MongoDB service:
     ```bash
     mongod
     ```

2. **MongoDB Atlas Setup (Alternative)**
   - Log in to MongoDB Atlas
   - Create a new cluster (free tier available)
   - Set up database access:
     1. Create a database user
     2. Set a secure password
   - Set up network access:
     1. Add your IP address to whitelist
     2. Or allow access from anywhere (not recommended for production)
   - Get your connection string and update MONGODB_URI in .env

## Step 4: Running the Application

1. **Development Mode**
   ```bash
   # Start both frontend and backend (from project root)
   npm run dev

   # Or start them separately:
   # Terminal 1 (Backend):
   node api/index.js

   # Terminal 2 (Frontend):
   cd client
   npm start
   ```

2. **Accessing the Application**
   - Frontend: http://localhost:3000
   - Backend API: http://localhost:3000/api

## Step 5: Initial Setup

1. **Create admin user**
   - Use the `/api/setup` endpoint with your ADMIN_TOKEN
   - Example using curl:
     ```bash
     curl -X POST http://localhost:3000/api/setup \
       -H "Content-Type: application/json" \
       -H "Authorization: Bearer your_admin_token_here" \
       -d '{"email":"admin@example.com","password":"secure_password"}'
     ```

2. **Configure Integrations**
   - Set up Google Business Profile integration
   - Follow prompts in the dashboard for additional platform integrations

## Common Issues and Troubleshooting

1. **MongoDB Connection Issues**
   - Verify MongoDB is running
   - Check connection string format
   - Ensure network/firewall allows MongoDB connection

2. **Node.js Version Conflicts**
   - Use `nvm` (Node Version Manager) to switch to the correct version
   - Check .nvmrc file for required version

3. **Package Installation Errors**
   - Clear npm cache: `npm cache clean --force`
   - Delete node_modules and package-lock.json
   - Run `npm install` again

4. **Environment Variable Issues**
   - Ensure all required variables are set in .env
   - No spaces around = in .env file
   - Check for proper string lengths (encryption keys)

## Security Notes

1. Never commit .env file to version control
2. Use strong, unique passwords for all services
3. Regularly update dependencies for security patches
4. Monitor application logs for suspicious activity

## Additional Resources

- MongoDB Documentation: https://docs.mongodb.com/
- Node.js Documentation: https://nodejs.org/docs/
- React Documentation: https://reactjs.org/docs/
- Vercel Documentation: https://vercel.com/docs