# ⚡ Quick Deploy Guide - 30 Minutes to Live!

## 🎯 Your Setup
- **Frontend**: Vercel (yourdomain.com)
- **Admin**: Vercel (admin.yourdomain.com)
- **Backend**: Render
- **Database**: Railway MySQL (Keep existing MySQL)

---

## 📝 Step-by-Step (30 Minutes)

### ⏱️ Step 1: Database (5 min) - Use Railway

1. Go to [Railway.app](https://railway.app)
2. Sign in with GitHub
3. **New Project** → **Provision MySQL**
4. Copy connection details:
   ```
   Host: containers-us-west-xxx.railway.app
   Port: 6379
   User: root
   Password: [shown in Railway]
   Database: railway
   ```
5. **Import your database**:
   - Download Railway CLI or use their web terminal
   - Run: `mysql -h HOST -u root -p DATABASE < complete_setup.sql`

---

### ⏱️ Step 2: Backend on Render (10 min)

1. Go to [Render.com](https://render.com)
2. **New** → **Web Service**
3. Connect: `am-fashions/am-with-emailjs`
4. Settings:
   ```
   Name: am-fashions-backend
   Root Directory: admin-dashboard/server
   Build: npm install
   Start: npm start
   ```
5. **Environment Variables** (copy-paste):
   ```
   DB_HOST=[Railway host]
   DB_USER=root
   DB_PASSWORD=[Railway password]
   DB_NAME=railway
   DB_PORT=6379
   PORT=5000
   NODE_ENV=production
   JWT_SECRET=your-random-secret-123456
   EMAIL_USER=madasumiteesh@gmail.com
   EMAIL_PASSWORD=mnfc xdxe ojpi rtzf
   ADMIN_EMAIL=madasumiteesh@gmail.com
   FRONTEND_URL=https://yourdomain.com
   ADMIN_URL=https://admin.yourdomain.com
   ```
6. **Create Web Service**
7. Copy your backend URL: `https://am-fashions-backend.onrender.com`

---

### ⏱️ Step 3: Main Website on Vercel (5 min)

1. Go to [Vercel.com](https://vercel.com)
2. **Import Project**
3. Select: `am-fashions/am-with-emailjs`
4. Settings:
   ```
   Framework: Create React App
   Root: ./
   Build: npm run build
   Output: build
   ```
5. **Environment Variable**:
   ```
   REACT_APP_API_URL=https://am-fashions-backend.onrender.com/api
   ```
6. **Deploy**
7. Copy URL: `https://am-fashions.vercel.app`

---

### ⏱️ Step 4: Admin Dashboard on Vercel (5 min)

1. In Vercel → **Add New Project**
2. Same repo: `am-fashions/am-with-emailjs`
3. Settings:
   ```
   Framework: Vite
   Root: admin-dashboard/client
   Build: npm run build
   Output: dist
   ```
4. **Environment Variable**:
   ```
   VITE_API_URL=https://am-fashions-backend.onrender.com/api
   ```
5. **Deploy**
6. Copy URL: `https://am-fashions-admin.vercel.app`

---

### ⏱️ Step 5: Connect Your Domain (5 min)

#### Main Website (yourdomain.com)
1. Vercel main project → **Settings** → **Domains**
2. Add: `yourdomain.com`
3. In your domain registrar, add:
   ```
   Type: A
   Name: @
   Value: 76.76.21.21
   ```

#### Admin Dashboard (admin.yourdomain.com)
1. Vercel admin project → **Settings** → **Domains**
2. Add: `admin.yourdomain.com`
3. In your domain registrar, add:
   ```
   Type: CNAME
   Name: admin
   Value: cname.vercel-dns.com
   ```

---

## ✅ Quick Test

1. **Backend**: Visit `https://am-fashions-backend.onrender.com/api/health`
   - Should show: `{"status":"OK"}`

2. **Main Website**: Visit `https://yourdomain.com`
   - Browse products
   - Add to cart
   - Try checkout

3. **Admin**: Visit `https://admin.yourdomain.com`
   - Try login
   - Check email for approval

---

## 🔧 If Something Breaks

### Backend Error?
- Check Render logs
- Verify database connection
- Check environment variables

### Frontend Error?
- Check browser console (F12)
- Verify API URL is correct
- Check CORS settings

### Database Error?
- Verify Railway MySQL is running
- Check connection details
- Test connection from Render

---

## 📋 Checklist

- [ ] Railway MySQL created
- [ ] Database imported
- [ ] Backend deployed to Render
- [ ] Main website deployed to Vercel
- [ ] Admin deployed to Vercel
- [ ] Domain connected
- [ ] Admin subdomain connected
- [ ] Tested main website
- [ ] Tested admin dashboard
- [ ] Email working

---

## 🎉 Done!

Your URLs:
```
Main:  https://yourdomain.com
Admin: https://admin.yourdomain.com
API:   https://am-fashions-backend.onrender.com
```

**Time taken**: ~30 minutes
**Cost**: $5/month (Railway MySQL)
**Status**: ✅ LIVE!

---

## 💡 Pro Tips

1. **Free Render**: Spins down after 15 min of inactivity (first request takes 30s)
2. **Upgrade Render**: $7/month for always-on
3. **Vercel**: Automatic HTTPS and CDN
4. **Railway**: Easy database management
5. **Backups**: Set up automated database backups

---

## 🆘 Emergency Contacts

- **Vercel Support**: https://vercel.com/support
- **Render Support**: https://render.com/docs
- **Railway Support**: https://railway.app/help

**Your Email**: madasumiteesh@gmail.com
