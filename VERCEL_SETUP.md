# Vercel Deployment Setup Guide

## Project Structure
```
backend/      → Node.js/Express API (serverless functions)
frontend/     → React app
```

## Step 1: Backend Setup

### 1.1 Update backend code for Vercel

Create `api/index.js` folder structure (Vercel requires this):

```bash
mkdir backend/api
mv backend/index.js backend/api/index.js
```

Update `backend/api/index.js` - add at the end:

```javascript
module.exports = app;
```

### 1.2 Update vercel.json for backend

The `backend/vercel.json` file is configured for serverless deployment.

### 1.3 Add environment variables to Vercel Dashboard

Go to Vercel Dashboard → Your Project → Settings → Environment Variables

Add these variables:
- `MONGODB_URI` - Your MongoDB Atlas connection string
- `FRONTEND_URL` - Your frontend Vercel URL (e.g., https://yourapp.vercel.app)
- `EMAIL_USER` - Gmail address
- `EMAIL_PASSWORD` - Gmail App Password (not your regular password)
- `NODE_ENV` - production

## Step 2: Frontend Setup

### 2.1 Update API calls in App.js

Find the axios call and update it to use the environment variable:

```javascript
const API_URL = process.env.REACT_APP_API_URL || 'http://localhost:3001'

// In your send function:
axios.post(`${API_URL}/sendmail`, { msg, email })
```

### 2.2 Add environment variables to Vercel

Go to Vercel Dashboard → Your Frontend Project → Settings → Environment Variables

Add:
- `REACT_APP_API_URL` - Your backend Vercel URL (e.g., https://backend-url.vercel.app)

## Step 3: Deploy to Vercel

### Backend Deployment:
```bash
cd backend
vercel deploy --prod
```

### Frontend Deployment:
```bash
cd frontend
vercel deploy --prod
```

## Step 4: Getting Your URLs

After deployment:
- **Backend URL**: Will be shown in Vercel dashboard (e.g., yourbulkmail-backend.vercel.app)
- **Frontend URL**: Will be shown in Vercel dashboard (e.g., yourbulkmail.vercel.app)

## Step 5: Update Environment Variables

Once you have both URLs:

1. Go to backend project settings → Environment Variables
   - Update `FRONTEND_URL` with your frontend URL

2. Go to frontend project settings → Environment Variables
   - Update `REACT_APP_API_URL` with your backend URL

3. Redeploy both (commit a small change or click redeploy)

## Important Notes

### Gmail Setup (for Email)
1. Enable 2-factor authentication on your Gmail account
2. Generate App Password: https://myaccount.google.com/apppasswords
3. Use the App Password (16 characters) as `EMAIL_PASSWORD`

### MongoDB Connection
- Use MongoDB Atlas
- Get connection string from Atlas dashboard
- Add Vercel IP to MongoDB Atlas IP whitelist (or allow all: 0.0.0.0/0)

### CORS Configuration
- Backend already has CORS setup with `FRONTEND_URL`
- Make sure `FRONTEND_URL` matches exactly (including protocol)

## File Structure After Setup

```
backend/
├── api/
│   └── index.js
├── vercel.json
├── .env.example
├── package.json
└── ...

frontend/
├── src/
│   └── App.js (update API URL)
├── .env.local
├── .env.production
├── package.json
└── ...
```

## Troubleshooting

### CORS Error
- Check `FRONTEND_URL` environment variable matches your deployed frontend URL

### MongoDB Connection Error
- Verify connection string is correct
- Check IP whitelist in MongoDB Atlas (should include 0.0.0.0/0 for Vercel)

### Email Not Sending
- Verify Gmail App Password is correct
- Check email credentials in Vercel environment variables
- Review Vercel logs for errors

### API URL Not Working
- Ensure `REACT_APP_API_URL` is set correctly in frontend
- The environment variable must start with `REACT_APP_` for Create React App

## Useful Commands

```bash
# Check Vercel environment variables
vercel env ls

# View logs
vercel logs

# Deploy specific environment
vercel deploy --prod

# List deployments
vercel list
```
