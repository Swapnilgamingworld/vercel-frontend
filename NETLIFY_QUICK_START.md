# Netlify Deployment Quick Start

Follow these steps to deploy your College Management System:

## Quick Checklist

- [ ] MongoDB Atlas account created & connection string copied
- [ ] Backend pushed to GitHub
- [ ] Backend deployed to Render
- [ ] Frontend `.env.production` updated with backend URL
- [ ] Frontend deployed to Netlify
- [ ] CORS configured in backend
- [ ] All features tested

## Step 1: Set Up Database (5 min)

1. Visit [mongodb.com/cloud/atlas](https://mongodb.com/cloud/atlas)
2. Create free account → Create M0 cluster
3. Create database user
4. Get connection string: `mongodb+srv://user:pass@cluster.mongodb.net/college-management`
5. **Save this - you'll need it for backend!**

## Step 2: Deploy Backend to Render (10 min)

1. Push your backend code to GitHub
2. Go to [render.com](https://render.com) → Sign in with GitHub
3. Click "New+" → "Web Service"
4. Select your GitHub repo
5. Fill in settings:
   - **Name**: college-management-backend
   - **Build Command**: `npm install`
   - **Start Command**: `npm start`
6. Add Environment Variables:
   ```
   MONGO_URI=mongodb+srv://user:pass@cluster.mongodb.net/college-management
   FRONTEND_URL=https://your-frontend.netlify.app
   NODE_ENV=production
   JWT_SECRET=any-random-secret-key
   ```
7. Click "Deploy Web Service"
8. **Wait for deploy** → Copy your URL (e.g., `https://college-management-backend.onrender.com`)

## Step 3: Update Frontend Environment

1. Open `frontend/.env.production`
2. Replace URL:
   ```
   REACT_APP_API_URL=https://college-management-backend.onrender.com/api
   ```
3. Save the file

## Step 4: Deploy Frontend to Netlify (10 min)

**Option A: Simple (Recommended)**
1. Go to [netlify.com](https://netlify.com) → Sign up with GitHub
2. Click "Add new site" → "Import an existing project"
3. Select your frontend folder
4. Netlify auto-detects settings (build: `npm run build`, publish: `build`)
5. Add environment variable:
   - Key: `REACT_APP_API_URL`
   - Value: `https://your-backend.onrender.com/api`
6. Click "Deploy site"
7. **Done!** Your site is live at `https://[your-site].netlify.app`

**Option B: Via CLI**
```bash
cd frontend
npm install -g netlify-cli
netlify login
netlify init
netlify deploy --prod
```

## Testing

1. Open your Netlify URL in browser
2. Try logging in with test credentials
3. Check browser console (F12) for errors
4. Backend may take 30-60 seconds on first request (free tier cold start)

## Need Help?

### Check Backend Logs
- Render Dashboard → Your app → "Logs" tab
- Look for MongoDB connection errors

### Check Frontend Logs
- Netlify Dashboard → Your site → "Deploys" tab → Click latest deploy
- Check "Deploy Log" for build errors

### Common Issues

**Frontend can't reach backend:**
- Verify URL in `.env.production`
- Check Render backend is running
- Clear browser cache (Ctrl+Shift+Delete)

**Backend deployment fails:**
- Check `package.json` has all dependencies
- Verify MongoDB connection string is correct
- Check environment variables are set

**Blank page after deploy:**
- Clear Netlify cache: Site settings → Deploys → Clear cache and redeploy

## Performance Tips

- Backend may be slow on first request (Render free tier)
- Monitor MongoDB usage (free tier has limits)
- Consider upgrading tiers for production

---

**Need the full guide?** See [DEPLOYMENT_GUIDE.md](DEPLOYMENT_GUIDE.md)
