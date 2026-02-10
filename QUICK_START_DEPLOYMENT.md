# ⚡ Quick Start - Deploy in 30 Minutes

## 🎯 What You Need

1. **GitHub Account** - Your code is already there ✅
2. **Vercel Account** - Sign up at [vercel.com](https://vercel.com) (use GitHub)
3. **Render Account** - Sign up at [render.com](https://render.com) (use GitHub)
4. **Railway Account** - Sign up at [railway.app](https://railway.app) (use GitHub)

---

## 📝 Step-by-Step (30 minutes)

### STEP 1: Database (5 minutes)

1. Go to [railway.app](https://railway.app)
2. Click **New Project** → **Provision MySQL**
3. Click MySQL → **Variables** tab
4. **Copy these 5 values** (keep them handy):
   - MYSQLHOST
   - MYSQLUSER
   - MYSQLPASSWORD
   - MYSQLDATABASE
   - MYSQLPORT

---

### STEP 2: Backend API (10 minutes)

1. Go to [render.com/dashboard](https://dashboard.render.com)
2. Click **New +** → **Web Service**
3. Connect your GitHub repo: `am-fashions/am-with-emailjs`
4. Fill in:
   ```
   Name: am-fashions-backend
   Root Directory: admin-dashboard/server
   Build Command: npm install
   Start Command: npm start
   ```
5. Click **Advanced** → Add these environment variables:
   ```
   DB_HOST=<paste MYSQLHOST from Railway>
   DB_USER=<paste MYSQLUSER from Railway>
   DB_PASSWORD=<paste MYSQLPASSWORD from Railway>
   DB_NAME=<paste MYSQLDATABASE from Railway>
   DB_PORT=<paste MYSQLPORT from Railway>
   PORT=5000
   NODE_ENV=production
   JWT_SECRET=my-super-secret-key-12345
   EMAIL_USER=madasumiteesh@gmail.com
   EMAIL_PASSWORD=mnfc xdxe ojpi rtzf
   ADMIN_EMAIL=madasumiteesh@gmail.com
   FRONTEND_URL=https://yourdomain.com
   ADMIN_URL=https://admin.yourdomain.com
   ```
6. Click **Create Web Service**
7. Wait 5-10 minutes for deployment
8. **Copy your backend URL**: `https://am-fashions-backend.onrender.com`

---

### STEP 3: Main Website (7 minutes)

1. **First, update the API URL**:
   - Open file: `am-with-emailjs/.env.production`
   - Replace with your Render URL:
     ```
     REACT_APP_API_URL=https://am-fashions-backend.onrender.com/api
     ```
   - Save and commit:
     ```bash
     git add .
     git commit -m "Update production API URL"
     git push origin main
     ```

2. Go to [vercel.com/dashboard](https://vercel.com/dashboard)
3. Click **Add New...** → **Project**
4. Import: `am-fashions/am-with-emailjs`
5. Configure:
   ```
   Framework: Create React App
   Root Directory: ./ (leave blank)
   Build Command: npm run build
   Output Directory: build
   ```
6. Add environment variable:
   ```
   REACT_APP_API_URL=https://am-fashions-backend.onrender.com/api
   ```
7. Click **Deploy**
8. Wait 3-5 minutes
9. **Your website is live!** 🎉

---

### STEP 4: Admin Dashboard (7 minutes)

1. **First, update the API URL**:
   - Open: `am-with-emailjs/admin-dashboard/client/.env.production`
   - Replace with your Render URL:
     ```
     VITE_API_URL=https://am-fashions-backend.onrender.com/api
     ```
   - Save and commit:
     ```bash
     git add .
     git commit -m "Update admin API URL"
     git push origin main
     ```

2. Go to [vercel.com/dashboard](https://vercel.com/dashboard)
3. Click **Add New...** → **Project**
4. Import SAME repo: `am-fashions/am-with-emailjs`
5. Configure:
   ```
   Framework: Vite
   Root Directory: admin-dashboard/client
   Build Command: npm run build
   Output Directory: dist
   ```
6. Add environment variable:
   ```
   VITE_API_URL=https://am-fashions-backend.onrender.com/api
   ```
7. Click **Deploy**
8. **Your admin dashboard is live!** 🎉

---

### STEP 5: Import Database (5 minutes)

1. Export your local database:
   ```bash
   mysqldump -u root ecommerce_admin > database_backup.sql
   ```

2. Go to Railway → Your MySQL service → **Data** tab
3. Click **Query** → Paste your SQL or use Railway CLI to import

**OR** run the setup scripts:
- `admin-dashboard/database/complete_setup.sql`
- `admin-dashboard/database/seed.sql`

---

### STEP 6: Update CORS (2 minutes)

1. Go to Render → Your backend service
2. **Environment** tab
3. Update these two variables with your actual Vercel URLs:
   ```
   FRONTEND_URL=https://your-website.vercel.app
   ADMIN_URL=https://your-admin.vercel.app
   ```
4. Save (auto-redeploys)

---

## ✅ Test Your Deployment

### Test Backend:
Visit: `https://your-backend.onrender.com/api/health`
Should see: `{"status":"OK"}`

### Test Website:
1. Visit your Vercel website URL
2. Browse products
3. Add to cart
4. Place order

### Test Admin:
1. Visit your Vercel admin URL
2. Try to login
3. Check email for approval
4. Approve and login

---

## 🎉 You're Done!

Your URLs:
```
Website: https://your-project.vercel.app
Admin:   https://your-admin.vercel.app
API:     https://your-backend.onrender.com
```

---

## 🔥 Pro Tips

1. **Render Free Tier**: Backend sleeps after 15 min inactivity
   - First request takes 30-60 seconds to wake up
   - Upgrade to $7/month for always-on

2. **Auto-Deploy**: Push to GitHub = auto-deploy
   ```bash
   git add .
   git commit -m "Update"
   git push
   ```

3. **Custom Domain**: 
   - Vercel → Settings → Domains
   - Add your domain
   - Follow DNS instructions

4. **View Logs**:
   - Render: Dashboard → Your service → Logs
   - Vercel: Dashboard → Your project → Deployments → View logs

---

## 🆘 Common Issues

**"Cannot connect to database"**
→ Check Railway MySQL is running
→ Verify environment variables in Render

**CORS errors**
→ Update FRONTEND_URL and ADMIN_URL in Render
→ Make sure no trailing slashes

**Build fails on Vercel**
→ Check build logs
→ Verify package.json has all dependencies
→ Try deploying again

**Backend returns 404**
→ Check Root Directory is set correctly
→ Verify Start Command is `npm start`

---

## 💰 Cost

- **Vercel**: FREE (2 projects)
- **Render**: FREE (with sleep)
- **Railway**: $5/month (MySQL)
- **Total**: $5/month

---

## 📞 Help

Full guide: See `DEPLOYMENT_STEPS.md`
Contact: madasumiteesh@gmail.com

**Happy Deploying! 🚀**
