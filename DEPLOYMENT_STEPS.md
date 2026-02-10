# 🚀 Quick Deployment Guide - Vercel + Render

Follow these steps in order to deploy your application.

---

## ✅ Prerequisites

1. GitHub repository: https://github.com/am-fashions/am-with-emailjs
2. Accounts (sign up with GitHub):
   - [ ] [Vercel](https://vercel.com) - For frontend hosting
   - [ ] [Render](https://render.com) - For backend API
   - [ ] [Railway](https://railway.app) or [PlanetScale](https://planetscale.com) - For MySQL database

---

## STEP 1: Set Up MySQL Database

### Option A: Railway (Recommended - Easy)

1. Go to [Railway.app](https://railway.app)
2. Click **New Project** → **Provision MySQL**
3. Click on MySQL service → **Variables** tab
4. Copy these values (you'll need them):
   ```
   MYSQLHOST
   MYSQLUSER
   MYSQLPASSWORD
   MYSQLDATABASE
   MYSQLPORT
   ```

### Option B: PlanetScale (Free Tier)

1. Go to [PlanetScale](https://planetscale.com)
2. Create new database: `am-fashions-db`
3. Get connection details from **Connect** button
4. Copy connection string

### Import Your Database

1. Export your local database:
   ```bash
   mysqldump -u root ecommerce_admin > database_backup.sql
   ```

2. Import to Railway/PlanetScale using their CLI or web interface

---

## STEP 2: Deploy Backend to Render

### 2.1 Prepare Backend

Your backend is already configured! Files created:
- ✅ `.env.example` - Template for environment variables
- ✅ `package.json` - Has correct start script

### 2.2 Deploy to Render

1. **Go to [Render Dashboard](https://dashboard.render.com)**

2. **Click "New +" → "Web Service"**

3. **Connect GitHub Repository**:
   - Select: `am-fashions/am-with-emailjs`
   - Click **Connect**

4. **Configure Service**:
   ```
   Name: am-fashions-backend
   Region: Choose closest to you
   Branch: main
   Root Directory: admin-dashboard/server
   Runtime: Node
   Build Command: npm install
   Start Command: npm start
   Instance Type: Free
   ```

5. **Add Environment Variables** (Click "Advanced" → "Add Environment Variable"):

   Copy these and update with your database values:
   ```
   DB_HOST=your-railway-mysql-host
   DB_USER=your-railway-mysql-user
   DB_PASSWORD=your-railway-mysql-password
   DB_NAME=ecommerce_admin
   DB_PORT=3306
   PORT=5000
   NODE_ENV=production
   JWT_SECRET=your-super-secret-jwt-key-change-this
   EMAIL_USER=madasumiteesh@gmail.com
   EMAIL_PASSWORD=mnfc xdxe ojpi rtzf
   ADMIN_EMAIL=madasumiteesh@gmail.com
   FRONTEND_URL=https://yourdomain.com
   ADMIN_URL=https://admin.yourdomain.com
   ```

   **Important**: Generate a strong JWT_SECRET:
   ```bash
   node -e "console.log(require('crypto').randomBytes(32).toString('hex'))"
   ```

6. **Click "Create Web Service"**

7. **Wait for deployment** (5-10 minutes)

8. **Copy your backend URL**: `https://am-fashions-backend.onrender.com`

9. **Test it**: Visit `https://am-fashions-backend.onrender.com/api/health`
   - Should return: `{"status":"OK"}`

---

## STEP 3: Deploy Main Website to Vercel

### 3.1 Update Environment Variables

1. **Edit `.env.production`** in root folder:
   ```env
   REACT_APP_API_URL=https://am-fashions-backend.onrender.com/api
   ```
   Replace with your actual Render backend URL.

### 3.2 Deploy to Vercel

1. **Go to [Vercel Dashboard](https://vercel.com/dashboard)**

2. **Click "Add New..." → "Project"**

3. **Import Git Repository**:
   - Select: `am-fashions/am-with-emailjs`
   - Click **Import**

4. **Configure Project**:
   ```
   Framework Preset: Create React App
   Root Directory: ./ (leave as root)
   Build Command: npm run build
   Output Directory: build
   Install Command: npm install
   ```

5. **Add Environment Variables**:
   ```
   REACT_APP_API_URL=https://am-fashions-backend.onrender.com/api
   ```

6. **Click "Deploy"**

7. **Wait for deployment** (3-5 minutes)

8. **Your website URL**: `https://am-fashions.vercel.app`

### 3.3 Add Custom Domain (Optional)

1. In Vercel project → **Settings** → **Domains**
2. Add your domain: `yourdomain.com`
3. Follow DNS instructions provided by Vercel
4. Wait for DNS propagation (5-60 minutes)

---

## STEP 4: Deploy Admin Dashboard to Vercel

### 4.1 Update Environment Variables

1. **Edit `admin-dashboard/client/.env.production`**:
   ```env
   VITE_API_URL=https://am-fashions-backend.onrender.com/api
   ```

### 4.2 Deploy to Vercel

1. **In Vercel Dashboard** → **Add New...** → **Project**

2. **Import SAME Repository**: `am-fashions/am-with-emailjs`

3. **Configure Project**:
   ```
   Framework Preset: Vite
   Root Directory: admin-dashboard/client
   Build Command: npm run build
   Output Directory: dist
   Install Command: npm install
   ```

4. **Add Environment Variables**:
   ```
   VITE_API_URL=https://am-fashions-backend.onrender.com/api
   ```

5. **Click "Deploy"**

6. **Your admin URL**: `https://am-fashions-admin.vercel.app`

### 4.3 Add Custom Subdomain (Optional)

1. In Vercel admin project → **Settings** → **Domains**
2. Add subdomain: `admin.yourdomain.com`
3. Add DNS CNAME record:
   ```
   Type: CNAME
   Name: admin
   Value: cname.vercel-dns.com
   ```

---

## STEP 5: Update Backend CORS

After deploying, update CORS in Render:

1. **Go to Render Dashboard** → Your backend service
2. **Environment** tab
3. **Update these variables**:
   ```
   FRONTEND_URL=https://yourdomain.com
   ADMIN_URL=https://admin.yourdomain.com
   ```
   (Use your actual Vercel URLs)

4. **Save Changes** - Render will auto-redeploy

---

## STEP 6: Test Everything

### Test Main Website
- [ ] Visit your website URL
- [ ] Browse products
- [ ] Add items to cart
- [ ] Place an order
- [ ] Upload payment screenshot
- [ ] Check if order email is received

### Test Admin Dashboard
- [ ] Visit admin URL
- [ ] Login (request approval)
- [ ] Check email for approval link
- [ ] Approve login
- [ ] View dashboard stats
- [ ] Check payment verifications
- [ ] View orders and customers

### Test Backend API
- [ ] Visit: `https://your-backend.onrender.com/api/health`
- [ ] Should return: `{"status":"OK","timestamp":"...","database":"Connected"}`

---

## 🎯 Final URLs

After deployment, you'll have:

```
Main Website:     https://am-fashions.vercel.app (or yourdomain.com)
Admin Dashboard:  https://am-fashions-admin.vercel.app (or admin.yourdomain.com)
Backend API:      https://am-fashions-backend.onrender.com
Database:         Railway/PlanetScale MySQL
```

---

## 🔧 Troubleshooting

### Backend shows "Database connection failed"
- Check environment variables in Render
- Verify database credentials are correct
- Ensure database allows external connections

### CORS errors in browser console
- Update FRONTEND_URL and ADMIN_URL in Render
- Make sure URLs don't have trailing slashes
- Redeploy backend after changes

### "Cannot GET /api/..." errors
- Check backend is running: visit `/api/health`
- Verify API_URL in frontend .env files
- Check Render logs for errors

### Email not sending
- Verify Gmail app password is correct
- Check Render logs for email errors
- Test email endpoint: `/api/test-email`

### Vercel build fails
- Check build logs in Vercel dashboard
- Verify all dependencies are in package.json
- Try deploying again (sometimes it's a timeout)

---

## 💰 Cost Breakdown

### Free Tier (Perfect for starting):
- **Vercel**: Free (2 projects, unlimited bandwidth)
- **Render**: Free (750 hours/month, sleeps after 15 min inactivity)
- **Railway MySQL**: $5/month (500MB storage)
- **Total**: $5/month

### Notes:
- Render free tier sleeps after inactivity (first request takes 30-60 seconds)
- Upgrade to Render Starter ($7/month) for always-on backend
- Vercel free tier is generous for most small businesses

---

## 🎉 You're Live!

Congratulations! Your e-commerce platform is now deployed on:
- ✅ Professional hosting infrastructure
- ✅ Automatic SSL certificates
- ✅ Global CDN (Vercel)
- ✅ Scalable backend (Render)
- ✅ Production-ready database

---

## 📞 Need Help?

- **Vercel Docs**: https://vercel.com/docs
- **Render Docs**: https://render.com/docs
- **Railway Docs**: https://docs.railway.app

**Questions?** Contact: madasumiteesh@gmail.com

---

## 🔄 Updating Your Deployment

### To update main website:
```bash
git add .
git commit -m "Update website"
git push origin main
```
Vercel auto-deploys on push!

### To update admin dashboard:
Same as above - Vercel watches the repo and auto-deploys.

### To update backend:
```bash
git add .
git commit -m "Update backend"
git push origin main
```
Render auto-deploys on push!

---

## 📝 Post-Deployment Checklist

- [ ] All three services deployed successfully
- [ ] Database connected and populated
- [ ] Custom domain configured (if applicable)
- [ ] SSL certificates active (automatic)
- [ ] Email service tested and working
- [ ] Payment verification flow tested
- [ ] Admin approval system tested
- [ ] All environment variables set correctly
- [ ] CORS configured for production domains
- [ ] Backup strategy in place for database

**You're all set! 🚀**
