# College Management System - Netlify Deployment Guide

## Overview
This project is a full-stack MERN application:
- **Frontend**: React - Deploys to Netlify (Static Hosting)
- **Backend**: Express.js - Deploys to Render, Railway, or Heroku
- **Database**: MongoDB - Atlas (Cloud)

---

## STEP 1: Backend Deployment (Render.com - Recommended)

### 1.1 Setup MongoDB Atlas (Database)
1. Go to [MongoDB Atlas](https://www.mongodb.com/cloud/atlas)
2. Create a free account
3. Create a new cluster (M0 - Free tier)
4. Create a database user with a strong password
5. Add your IP to whitelist (or use 0.0.0.0 for anywhere)
6. Copy the connection string: `mongodb+srv://username:password@cluster.mongodb.net/college-management?retryWrites=true&w=majority`

### 1.2 Deploy Backend to Render

1. **Push backend to GitHub**
   ```bash
   cd backend
   git init
   git add .
   git commit -m "Initial backend commit"
   git remote add origin <your-github-repo-url>
   git push -u origin main
   ```

2. **Create Render Account**
   - Go to [render.com](https://render.com)
   - Sign up with GitHub
   - Click "New +" → Select "Web Service"
   - Connect your GitHub repository

3. **Configure Render**
   - **Name**: college-management-backend
   - **Runtime**: Node
   - **Build Command**: `npm install`
   - **Start Command**: `npm start`
   - **Environment Variables** (Add these):
     ```
     PORT=5000
     MONGO_URI=mongodb+srv://username:password@cluster.mongodb.net/college-management
     NODE_ENV=production
     ```

4. **Deploy** - Click "Deploy Web Service"
5. **Copy the Render URL** (e.g., `https://college-management-backend.onrender.com`)

---

## STEP 2: Frontend Deployment (Netlify)

### 2.1 Prepare Frontend
1. **Update API URL** - In [frontend/.env.production](frontend/.env.production):
   ```
   REACT_APP_API_URL=https://college-management-backend.onrender.com/api
   ```

2. **Create netlify.toml** in frontend root:
   ```toml
   [build]
   command = "npm run build"
   publish = "build"

   [[redirects]]
   from = "/*"
   to = "/index.html"
   status = 200
   ```

### 2.2 Create .env.production file
Create `frontend/.env.production`:
```
REACT_APP_API_URL=https://your-backend-url.onrender.com/api
```

### 2.3 Deploy to Netlify

**Option A: Via Netlify UI (Easiest)**
1. Go to [netlify.com](https://netlify.com)
2. Sign up with GitHub
3. Click "Add new site" → "Import an existing project"
4. Connect your GitHub repo
5. Select frontend folder as publish directory
6. Add environment variables:
   - `REACT_APP_API_URL=https://your-backend-url.onrender.com/api`
7. Deploy!

**Option B: Via Netlify CLI**
```bash
cd frontend
npm install -g netlify-cli
netlify login
netlify init
# Select manual settings
# Publish dir: build
# Build command: npm run build
netlify deploy --prod
```

---

## STEP 3: Configure CORS (Backend)

Update [backend/server.js](backend/server.js) for production:
```javascript
const corsOptions = {
  origin: process.env.FRONTEND_URL || 'http://localhost:3000',
  credentials: true,
};
app.use(cors(corsOptions));
```

Add to backend environment variables:
```
FRONTEND_URL=https://your-netlify-domain.netlify.app
```

---

## STEP 4: Additional Configuration

### 4.1 Create .gitignore files if not present

**backend/.gitignore**:
```
node_modules/
.env
.env.local
.DS_Store
```

**frontend/.gitignore**:
```
node_modules/
build/
.env.local
.DS_Store
npm-debug.log
```

### 4.2 Create backend .env template

Create `backend/.env.example`:
```
PORT=5000
MONGO_URI=mongodb+srv://username:password@cluster.mongodb.net/college-management
NODE_ENV=development
FRONTEND_URL=http://localhost:3000
JWT_SECRET=your-secret-key-here
```

---

## Testing After Deployment

1. **Test Backend**: Visit `https://your-backend-url.onrender.com`
2. **Test Frontend**: Visit `https://your-netlify-domain.netlify.app`
3. **Test API Call**: Try logging in - check network tab (F12) for API requests
4. **Check Logs**:
   - Render: Go to "Logs" tab
   - Netlify: Go to "Deploys" → "Deploy log"

---

## Troubleshooting

### Frontend can't reach backend
- [ ] Check `REACT_APP_API_URL` is correct
- [ ] Verify backend CORS settings
- [ ] Check browser console for errors (F12)

### Backend deployment fails
- [ ] Check build logs on Render
- [ ] Verify all dependencies in package.json
- [ ] Check MongoDB connection string

### Cold start delays
- This is normal on free tiers. Backend may take 30-60 seconds on first request.

---

## Monitoring & Maintenance

- **Monitor Backend**: Render dashboard → Logs
- **Monitor Frontend**: Netlify Analytics
- **Database**: MongoDB Atlas dashboard
- **Error Tracking**: Consider adding Sentry for error monitoring

---

## Alternative Backend Hosting Options

| Service | Pros | Cons |
|---------|------|------|
| **Render** | Free tier, easy setup | Cold starts |
| **Railway** | Good performance, $5 credit | Limited free tier |
| **Heroku** | Very popular | Paid (eco dynos discontinued) |
| **Fly.io** | Fast, reliable | Learning curve |
| **AWS** | Scalable | Complex setup |

---

## Next Steps

1. Set up MongoDB Atlas
2. Deploy backend to Render
3. Update frontend .env.production with backend URL
4. Deploy frontend to Netlify
5. Test all features thoroughly
6. Monitor logs for errors
