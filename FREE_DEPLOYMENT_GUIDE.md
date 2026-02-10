# 🆓 100% FREE Deployment Guide - No Credit Card Required!

Deploy your entire application for **$0/month** using free tiers.

---

## 🎯 What You'll Use (All FREE)

1. **Vercel** - Frontend hosting (FREE forever)
2. **Render** - Backend API + PostgreSQL Database (FREE with sleep)

**Total Cost: $0/month** 🎉

---

## 📝 Step-by-Step (35 minutes)

### STEP 1: Free PostgreSQL Database on Render (8 minutes)

Render offers **FREE PostgreSQL** (1GB storage) - no credit card needed!

#### 1.1 Create Render Account

1. Go to [render.com](https://render.com)
2. Click **Get Started** → **Sign in with GitHub**
3. Authorize Render

#### 1.2 Create PostgreSQL Database

1. In Render Dashboard, click **New +** → **PostgreSQL**
2. Configure:
   ```
   Name: am-fashions-db
   Database: amfashions
   User: amfashions
   Region: Choose closest to you
   PostgreSQL Version: 16 (latest)
   Instance Type: Free
   ```
3. Click **Create Database**
4. Wait 2-3 minutes for provisioning

#### 1.3 Get Connection Details

1. Click on your database
2. Scroll down to **Connections** section
3. Copy **Internal Database URL** (starts with `postgres://`)
4. Save this - you'll need it for backend!

Example:
```
postgres://amfashions:xxxxx@dpg-xxxxx-a.oregon-postgres.render.com/amfashions
```

#### 1.4 Import Your Database Schema

1. In Render database dashboard, click **Connect** → **External Connection**
2. Copy the **PSQL Command**
3. On your computer, run:
   ```bash
   # Connect to database
   psql postgres://your-connection-string-here
   
   # Then paste the contents of postgresql_setup.sql
   # Or run directly:
   psql postgres://your-connection-string-here < admin-dashboard/database/postgresql_setup.sql
   ```

**Don't have psql installed?** Use Render's **Shell** tab in the database dashboard to run SQL directly!

---

### STEP 2: Deploy Backend to Render (10 minutes)

Render offers **750 hours/month FREE** - more than enough!

#### 2.1 Create Render Account

1. Go to [render.com](https://render.com)
2. Click **Get Started** → **Sign in with GitHub**
3. Authorize Render

#### 2.2 Deploy Backend

1. Click **New +** → **Web Service**
2. Connect repository: `am-fashions/am-with-emailjs`
3. Configure:
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

#### 2.3 Add Environment Variables

Click **Advanced** → **Add Environment Variable**

Add these (replace with your Clever Cloud values):

```
DB_HOST=<your-clever-cloud-host>
DB_USER=<your-clever-cloud-user>
DB_PASSWORD=<your-clever-cloud-password>
DB_NAME=<your-clever-cloud-db>
DB_PORT=<your-clever-cloud-port>
PORT=5000
NODE_ENV=production
JWT_SECRET=my-super-secret-jwt-key-change-this-to-something-random
EMAIL_USER=madasumiteesh@gmail.com
EMAIL_PASSWORD=mnfc xdxe ojpi rtzf
ADMIN_EMAIL=madasumiteesh@gmail.com
FRONTEND_URL=https://yourdomain.com
ADMIN_URL=https://admin.yourdomain.com
```

**Generate a strong JWT_SECRET:**
```bash
node -e "console.log(require('crypto').randomBytes(32).toString('hex'))"
```

#### 2.4 Deploy

1. Click **Create Web Service**
2. Wait 5-10 minutes
3. Copy your backend URL: `https://am-fashions-backend.onrender.com`
4. Test it: Visit `https://am-fashions-backend.onrender.com/api/health`

**Note:** Free tier sleeps after 15 minutes of inactivity. First request takes 30-60 seconds to wake up.

---

### STEP 3: Deploy Main Website to Vercel (7 minutes)

Vercel is **100% FREE** for personal projects!

#### 3.1 Update API URL

1. Edit `am-with-emailjs/.env.production`:
   ```env
   REACT_APP_API_URL=https://am-fashions-backend.onrender.com/api
   ```
   (Replace with your actual Render URL)

2. Commit changes:
   ```bash
   git add .
   git commit -m "Update production API URL"
   git push origin main
   ```

#### 3.2 Deploy to Vercel

1. Go to [vercel.com](https://vercel.com)
2. Click **Sign Up** → **Continue with GitHub**
3. Click **Add New...** → **Project**
4. Import: `am-fashions/am-with-emailjs`
5. Configure:
   ```
   Framework Preset: Create React App
   Root Directory: ./ (leave blank)
   Build Command: npm run build
   Output Directory: build
   ```
6. Add Environment Variable:
   ```
   REACT_APP_API_URL=https://am-fashions-backend.onrender.com/api
   ```
7. Click **Deploy**
8. Wait 3-5 minutes
9. **Your website is live!** 🎉

Copy your URL: `https://am-fashions.vercel.app`

---

### STEP 4: Deploy Admin Dashboard to Vercel (7 minutes)

#### 4.1 Update API URL

1. Edit `am-with-emailjs/admin-dashboard/client/.env.production`:
   ```env
   VITE_API_URL=https://am-fashions-backend.onrender.com/api
   ```

2. Commit changes:
   ```bash
   git add .
   git commit -m "Update admin API URL"
   git push origin main
   ```

#### 4.2 Deploy to Vercel

1. In Vercel Dashboard → **Add New...** → **Project**
2. Import SAME repository: `am-fashions/am-with-emailjs`
3. Configure:
   ```
   Framework Preset: Vite
   Root Directory: admin-dashboard/client
   Build Command: npm run build
   Output Directory: dist
   ```
4. Add Environment Variable:
   ```
   VITE_API_URL=https://am-fashions-backend.onrender.com/api
   ```
5. Click **Deploy**
6. **Your admin dashboard is live!** 🎉

Copy your URL: `https://am-fashions-admin.vercel.app`

---

### STEP 5: Update Backend CORS (3 minutes)

1. Go to Render Dashboard → Your backend service
2. Click **Environment** tab
3. Update these variables with your actual Vercel URLs:
   ```
   FRONTEND_URL=https://am-fashions.vercel.app
   ADMIN_URL=https://am-fashions-admin.vercel.app
   ```
4. Click **Save Changes** (auto-redeploys)

---

### STEP 6: Test Everything (5 minutes)

#### Test Backend
Visit: `https://your-backend.onrender.com/api/health`
Should see: `{"status":"OK","timestamp":"...","database":"Connected"}`

#### Test Main Website
1. Visit your Vercel website URL
2. Browse products ✅
3. Add to cart ✅
4. Place order ✅
5. Upload payment screenshot ✅

#### Test Admin Dashboard
1. Visit your Vercel admin URL
2. Request login ✅
3. Check email for approval ✅
4. Approve login ✅
5. View dashboard ✅

---

## 🎉 You're Live - 100% FREE!

Your deployed URLs:
```
Main Website:     https://am-fashions.vercel.app
Admin Dashboard:  https://am-fashions-admin.vercel.app
Backend API:      https://am-fashions-backend.onrender.com
Database:         Clever Cloud MySQL (FREE)
```

**Total Cost: $0/month** 💰

---

## 🆓 Free Tier Limits

### Vercel (FREE Forever)
- ✅ Unlimited bandwidth
- ✅ Automatic SSL
- ✅ Global CDN
- ✅ 100GB bandwidth/month
- ✅ Auto-deploy on git push

### Render (FREE)
- ✅ 750 hours/month (more than enough)
- ⚠️ Sleeps after 15 min inactivity
- ⚠️ First request takes 30-60 seconds to wake
- ✅ Automatic SSL
- ✅ Auto-deploy on git push

### Clever Cloud MySQL (FREE)
- ✅ 256MB storage
- ✅ Shared CPU
- ✅ No credit card required
- ✅ Perfect for small projects

---

## 💡 Alternative FREE Database Options

If Clever Cloud doesn't work, try these:

### Option 1: FreeSQLDatabase.com
1. Go to [freesqldatabase.com](https://www.freesqldatabase.com)
2. Sign up (no credit card)
3. Create MySQL database (5MB free)
4. Get connection details

### Option 2: db4free.net
1. Go to [db4free.net](https://www.db4free.net)
2. Sign up
3. Create database (200MB free)
4. Use connection details

### Option 3: Aiven (FREE Trial)
1. Go to [aiven.io](https://aiven.io)
2. Sign up with GitHub
3. Create MySQL service (FREE trial - 30 days)
4. No credit card for trial

### Option 4: Render PostgreSQL (Convert from MySQL)
1. Render offers FREE PostgreSQL (1GB)
2. You'd need to convert your MySQL schema to PostgreSQL
3. Change `mysql2` to `pg` in your code

---

## 🔧 Dealing with Render Sleep (FREE Tier)

Your backend sleeps after 15 minutes. Here's how to handle it:

### Option 1: Accept the Sleep (Recommended)
- First request takes 30-60 seconds
- Subsequent requests are instant
- Perfect for low-traffic sites

### Option 2: Keep-Alive Ping (Free)
Use a free service to ping your backend every 14 minutes:

1. **UptimeRobot** (free):
   - Go to [uptimerobot.com](https://uptimerobot.com)
   - Add monitor: `https://your-backend.onrender.com/api/health`
   - Check every 5 minutes
   - Keeps your backend awake!

2. **Cron-Job.org** (free):
   - Go to [cron-job.org](https://cron-job.org)
   - Create job to ping your backend every 14 minutes

### Option 3: Client-Side Warmup
Add this to your frontend to wake backend on page load:

```javascript
// In your main App.jsx
useEffect(() => {
  // Warm up backend on app load
  fetch('https://your-backend.onrender.com/api/health')
    .catch(() => {}); // Ignore errors
}, []);
```

---

## 🚀 Auto-Deploy Setup

Both Vercel and Render auto-deploy when you push to GitHub!

```bash
# Make changes to your code
git add .
git commit -m "Update feature"
git push origin main

# That's it! Both services auto-deploy 🎉
```

---

## 🔒 Security Tips (FREE)

1. **Change JWT_SECRET**: Use a strong random string
   ```bash
   node -e "console.log(require('crypto').randomBytes(32).toString('hex'))"
   ```

2. **Environment Variables**: Never commit `.env` files
   - Already in `.gitignore` ✅

3. **Database Backups**: Export regularly
   ```bash
   mysqldump -h <host> -u <user> -p<password> <database> > backup.sql
   ```

4. **Monitor Usage**: Check your free tier limits
   - Vercel: Dashboard → Usage
   - Render: Dashboard → Usage
   - Clever Cloud: Dashboard → Metrics

---

## 📊 Performance Tips (FREE Tier)

### Optimize Images
```bash
# Install image optimizer
npm install --save-dev imagemin imagemin-mozjpeg imagemin-pngquant

# Compress images before deploying
```

### Enable Caching
Already configured in your Vercel setup! ✅

### Lazy Load Components
```javascript
// Use React.lazy for code splitting
const AdminDashboard = React.lazy(() => import('./pages/Dashboard'));
```

---

## 🆘 Troubleshooting

### "Backend takes too long to respond"
→ Normal on first request (Render waking up)
→ Use UptimeRobot to keep it awake
→ Or add loading message: "Waking up server..."

### "Database connection failed"
→ Check Clever Cloud database is active
→ Verify all 5 connection variables
→ Check database allows external connections

### "CORS errors"
→ Update FRONTEND_URL and ADMIN_URL in Render
→ Make sure URLs match exactly (no trailing slash)
→ Redeploy backend

### "Build fails on Vercel"
→ Check build logs in Vercel dashboard
→ Verify package.json has all dependencies
→ Try deploying again (sometimes it's a timeout)

---

## 💰 Cost Comparison

### Your FREE Setup:
```
Vercel:        $0/month
Render:        $0/month
Clever Cloud:  $0/month
Total:         $0/month ✅
```

### If You Upgrade Later:
```
Vercel Pro:    $20/month (optional)
Render Starter: $7/month (no sleep)
Railway MySQL:  $5/month (better performance)
Total:         $32/month
```

**Start free, upgrade when you need it!**

---

## 🎯 Next Steps

1. ✅ Deploy everything (follow steps above)
2. ✅ Test all features
3. ✅ Set up UptimeRobot to keep backend awake
4. ✅ Add custom domain (optional - Vercel makes it easy)
5. ✅ Monitor your usage
6. ✅ Backup your database regularly

---

## 📞 Support

- **Vercel Docs**: https://vercel.com/docs
- **Render Docs**: https://render.com/docs
- **Clever Cloud Docs**: https://www.clever-cloud.com/doc/

**Questions?** madasumiteesh@gmail.com

---

## 🎉 Congratulations!

You've deployed a full-stack e-commerce application with:
- ✅ Professional hosting
- ✅ Automatic SSL certificates
- ✅ Global CDN
- ✅ Auto-deploy on git push
- ✅ Email notifications
- ✅ Admin dashboard
- ✅ Payment verification system

**All for $0/month!** 🚀

---

## 📝 Quick Reference

### Your Deployment URLs
```
Website: https://am-fashions.vercel.app
Admin:   https://am-fashions-admin.vercel.app
API:     https://am-fashions-backend.onrender.com
```

### Important Commands
```bash
# Deploy updates
git add .
git commit -m "Update"
git push origin main

# Backup database
mysqldump -h <host> -u <user> -p<password> <db> > backup.sql

# Generate JWT secret
node -e "console.log(require('crypto').randomBytes(32).toString('hex'))"
```

### Free Services Used
- Vercel: Frontend hosting
- Render: Backend API
- Clever Cloud: MySQL database
- UptimeRobot: Keep backend awake (optional)

**Happy deploying! 🎉**
