# Environment Variables Reference

## Backend Environment Variables

### Development (backend/.env)
```
PORT=5000
MONGO_URI=mongodb://127.0.0.1:27017/college-management
NODE_ENV=development
FRONTEND_URL=http://localhost:3000
JWT_SECRET=dev-secret-key-for-local-testing
```

### Production (Render Environment Variables)
```
PORT=5000
MONGO_URI=mongodb+srv://username:password@cluster.mongodb.net/college-management?retryWrites=true&w=majority
NODE_ENV=production
FRONTEND_URL=https://your-site.netlify.app
JWT_SECRET=generate-a-strong-random-secret-here
```

---

## Frontend Environment Variables

### Development (frontend/.env.local)
```
REACT_APP_API_URL=http://localhost:5000/api
```

### Production (frontend/.env.production)
```
REACT_APP_API_URL=https://your-backend.onrender.com/api
```

---

## Getting Values

### MongoDB Connection String
1. Go to MongoDB Atlas Dashboard
2. Database → Connect
3. Choose "Drivers" → Node.js
4. Copy connection string
5. Replace `<password>` with your database user password
6. Replace `college-management` with your database name

**Format**: 
```
mongodb+srv://username:password@cluster0.xxxxx.mongodb.net/college-management?retryWrites=true&w=majority
```

### JWT Secret
Generate a strong random secret:
```bash
node -e "console.log(require('crypto').randomBytes(32).toString('hex'))"
```

### Frontend URL (for CORS)
- Development: `http://localhost:3000`
- Render: `https://your-app-name.onrender.com`
- Netlify: `https://your-site-name.netlify.app`

---

## Security Guidelines

1. **Never commit .env files** - Already in .gitignore
2. **Use strong JWT secrets** - Generate random 32-char strings
3. **Don't share connection strings** - Keep MongoDB credentials private
4. **Use environment variables** - Never hardcode secrets
5. **Review deployed variables** - Check Netlify & Render settings regularly

---

## MongoDB Atlas Security

### Whitelist IPs
- Render: Add Render's outbound IP → [Check here](https://docs.render.com/deploy-node-with-mongodb-atlas)
- Local testing: Add your local IP
- Anywhere (not recommended): Use `0.0.0.0/0` (if not handling sensitive data)

### Create Database User
1. Database Access → Add New Database User
2. Use strong password
3. Set role: `dbOwner` for full access
4. Use this user in connection string

---

## Troubleshooting Environment Variables

### Backend not connecting to database
- [ ] Check MONGO_URI format
- [ ] Verify username:password is correct
- [ ] Confirm IP is whitelisted in MongoDB Atlas
- [ ] Try connection string in MongoDB Compass to test

### Frontend API calls failing
- [ ] Confirm REACT_APP_API_URL is set
- [ ] Verify backend URL is accessible
- [ ] Check CORS_ORIGIN in backend matches frontend URL
- [ ] Test backend directly: curl https://your-backend.onrender.com

### Production issues
- [ ] Render Dashboard → Environment section → verify all vars set
- [ ] Netlify Site settings → Build & Deploy → Environment → verify REACT_APP_API_URL
- [ ] Clear Netlify cache and redeploy
