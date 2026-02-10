# 🚀 Complete Deployment Guide: Vercel + Render + MongoDB Atlas

## Overview
- **Main Website**: Vercel (yourdomain.com)
- **Admin Dashboard**: Vercel (admin.yourdomain.com or yourdomain.com/admin)
- **Backend API**: Render (api.yourdomain.com)
- **Database**: MongoDB Atlas (Free Tier)

---

## 📋 Prerequisites

1. ✅ GitHub repository: https://github.com/am-fashions/am-with-emailjs
2. ✅ Your custom domain
3. Accounts needed:
   - [ ] Vercel account (sign up with GitHub)
   - [ ] Render account (sign up with GitHub)
   - [ ] MongoDB Atlas account (free)

---

## PART 1: Set Up MongoDB Atlas (Database)

### Step 1: Create MongoDB Atlas Account

1. Go to [MongoDB Atlas](https://www.mongodb.com/cloud/atlas/register)
2. Sign up (free tier available)
3. Create a new cluster:
   - Choose **FREE** tier (M0)
   - Select region closest to you (e.g., Mumbai for India)
   - Cluster name: `am-fashions-cluster`

### Step 2: Configure Database Access

1. **Database Access** → **Add New Database User**
   - Username: `amfashions_admin`
   - Password: Generate a strong password (save it!)
   - Database User Privileges: **Read and write to any database**

2. **Network Access** → **Add IP Address**
   - Click **Allow Access from Anywhere** (0.0.0.0/0)
   - This allows Render to connect

### Step 3: Get Connection String

1. Click **Connect** on your cluster
2. Choose **Connect your application**
3. Copy the connection string:
   ```
   mongodb+srv://amfashions_admin:<password>@am-fashions-cluster.xxxxx.mongodb.net/?retryWrites=true&w=majority
   ```
4. Replace `<password>` with your actual password
5. Save this connection string - you'll need it!

### Step 4: Create Database

1. Click **Browse Collections**
2. **Add My Own Data**
3. Database name: `ecommerce_admin`
4. Collection name: `customers` (we'll add more later)

---

## PART 2: Migrate from MySQL to MongoDB

### Option A: Keep MySQL (Easier - Recommended)

Since you're already using MySQL and it's working, you can:
1. Use **Render's PostgreSQL** (free tier) or **Railway's MySQL**
2. Skip MongoDB migration
3. Keep your existing code

**If you want to keep MySQL, skip to PART 3 and use Railway or Render PostgreSQL instead.**

### Option B: Migrate to MongoDB (More Work)

If you want to use MongoDB, you'll need to:

1. **Install Mongoose** (MongoDB ODM):
```bash
cd admin-dashboard/server
npm install mongoose
```

2. **Update database connection** in `server/config/database.js`
3. **Convert SQL queries to MongoDB queries**
4. **Update all controllers**

**This requires significant code changes. I recommend keeping MySQL for now.**

---

## PART 3: Deploy Backend to Render

### Step 1: Prepare Backend for Deployment

1. **Create `render.yaml`** in `admin-dashboard/server/`:

```yaml
services:
  - type: web
    name: am-fashions-backend
    env: node
    buildCommand: npm install
    startCommand: npm start
    envVars:
      - key: NODE_ENV
        value: production
      - key: PORT
        value: 5000
```

2. **Update `package.json`** in `admin-dashboard/server/`:

Add this to scripts:
```json
"scripts": {
  "start": "node server.js",
  "dev": "nodemon server.js"
}
```

3. **Create `.env.example`** in `admin-dashboard/server/`:

```env
# Database (Use Railway MySQL or keep your existing MySQL)
DB_HOST=your-db-host
DB_USER=your-db-user
DB_PASSWORD=your-db-password
DB_NAME=ecommerce_admin
DB_PORT=3306

# Server
PORT=5000
NODE_ENV=production

# Security
JWT_SECRET=your-jwt-secret-here

# Email
EMAIL_USER=madasumiteesh@gmail.com
EMAIL_PASSWORD=mnfc xdxe ojpi rtzf
ADMIN_EMAIL=madasumiteesh@gmail.com

# Frontend URLs (Update after deployment)
FRONTEND_URL=https://yourdomain.com
ADMIN_URL=https://admin.yourdomain.com
```

### Step 2: Deploy to Render

1. **Go to [Render](https://render.com)**
2. **Sign in with GitHub**
3. **New** → **Web Service**
4. **Connect your repository**: `am-fashions/am-with-emailjs`
5. **Configure**:
   - Name: `am-fashions-backend`
   - Root Directory: `admin-dashboard/server`
   - Environment: `Node`
   - Build Command: `npm install`
   - Start Command: `npm start`
   - Instance Type: **Free**

6. **Add Environment Variables**:
   Click **Advanced** → **Add Environment Variable**
   
   Add all variables from `.env.example`:
   ```
   DB_HOST=your-mysql-host
   DB_USER=your-mysql-user
   DB_PASSWORD=your-mysql-password
   DB_NAME=ecommerce_admin
   DB_PORT=3306
   PORT=5000
   NODE_ENV=production
   JWT_SECRET=generate-random-secret-here
   EMAIL_USER=madasumiteesh@gmail.com
   EMAIL_PASSWORD=mnfc xdxe ojpi rtzf
   ADMIN_EMAIL=madasumiteesh@gmail.com
   FRONTEND_URL=https://yourdomain.com
   ADMIN_URL=https://admin.yourdomain.com
   ```

7. **Create Web Service**

8. **Wait for deployment** (5-10 minutes)

9. **Your backend URL**: `https://am-fashions-backend.onrender.com`

### Step 3: Set Up MySQL Database on Render

Since you're using MySQL, you have two options:

**Option A: Use Railway for MySQL (Recommended)**
1. Go to [Railway.app](https://railway.app)
2. New Project → Add MySQL
3. Copy connection details
4. Use in Render environment variables

**Option B: Use external MySQL hosting**
- [PlanetScale](https://planetscale.com) (Free tier)
- [Clever Cloud](https://www.clever-cloud.com) (Free tier)
- [FreeSQLDatabase](https://www.freesqldatabase.com)

---

## PART 4: Deploy Main Website to Vercel

### Step 1: Update API URLs

1. **Create `.env.production`** in root (`am-with-emailjs/`):

```env
REACT_APP_API_URL=https://am-fashions-backend.onrender.com/api
```

2. **Update API calls** to use environment variable:

In `src/services/api.js`:
```javascript
const API_BASE_URL = process.env.REACT_APP_API_URL || 'http://localhost:5000/api';
```

### Step 2: Deploy to Vercel

1. **Go to [Vercel](https://vercel.com)**
2. **Sign in with GitHub**
3. **Import Project**
4. **Select repository**: `am-fashions/am-with-emailjs`
5. **Configure**:
   - Framework Preset: **Create React App**
   - Root Directory: `./` (leave as root)
   - Build Command: `npm run build`
   - Output Directory: `build`
   - Install Command: `npm install`

6. **Environment Variables**:
   ```
   REACT_APP_API_URL=https://am-fashions-backend.onrender.com/api
   ```

7. **Deploy**

8. **Your website URL**: `https://am-fashions.vercel.app`

### Step 3: Connect Custom Domain

1. In Vercel project → **Settings** → **Domains**
2. **Add Domain**: `yourdomain.com`
3. **Add DNS Records** (in your domain registrar):

   **For root domain (yourdomain.com)**:
   ```
   Type: A
   Name: @
   Value: 76.76.21.21
   ```

   **For www subdomain**:
   ```
   Type: CNAME
   Name: www
   Value: cname.vercel-dns.com
   ```

4. **Wait for DNS propagation** (5-60 minutes)

---

## PART 5: Deploy Admin Dashboard to Vercel

### Step 1: Update API URLs

1. **Create `.env.production`** in `admin-dashboard/client/`:

```env
VITE_API_URL=https://am-fashions-backend.onrender.com/api
```

2. **Update API calls** in `admin-dashboard/client/src/services/api.js`:

```javascript
const API_BASE_URL = import.meta.env.VITE_API_URL || 'http://localhost:5000/api';
```

### Step 2: Deploy to Vercel

1. **In Vercel** → **Add New Project**
2. **Import same repository**: `am-fashions/am-with-emailjs`
3. **Configure**:
   - Framework Preset: **Vite**
   - Root Directory: `admin-dashboard/client`
   - Build Command: `npm run build`
   - Output Directory: `dist`
   - Install Command: `npm install`

4. **Environment Variables**:
   ```
   VITE_API_URL=https://am-fashions-backend.onrender.com/api
   ```

5. **Deploy**

6. **Your admin URL**: `https://am-fashions-admin.vercel.app`

### Step 3: Connect Custom Subdomain

1. In Vercel admin project → **Settings** → **Domains**
2. **Add Domain**: `admin.yourdomain.com`
3. **Add DNS Record** (in your domain registrar):

   ```
   Type: CNAME
   Name: admin
   Value: cname.vercel-dns.com
   ```

4. **Wait for DNS propagation**

---

## PART 6: Update Backend CORS

After deployment, update CORS in `admin-dashboard/server/server.js`:

```javascript
const allowedOrigins = [
  'https://yourdomain.com',
  'https://www.yourdomain.com',
  'https://admin.yourdomain.com',
  'http://localhost:3000',
  'http://localhost:3001'
];
```

**Commit and push changes**:
```bash
git add .
git commit -m "Update CORS for production domains"
git push origin main
```

Render will automatically redeploy.

---

## PART 7: Import Database to Production

### Option 1: Export and Import MySQL

1. **Export local database**:
```bash
mysqldump -u root ecommerce_admin > database_backup.sql
```

2. **Import to production MySQL**:
```bash
mysql -h your-production-host -u your-user -p your-database < database_backup.sql
```

### Option 2: Run SQL Scripts

Upload and run these files on your production database:
- `admin-dashboard/database/complete_setup.sql`
- `admin-dashboard/database/seed.sql`

---

## PART 8: Test Everything

### Test Main Website
- [ ] Visit `https://yourdomain.com`
- [ ] Browse products
- [ ] Add to cart
- [ ] Place order
- [ ] Upload payment screenshot
- [ ] Contact form works

### Test Admin Dashboard
- [ ] Visit `https://admin.yourdomain.com`
- [ ] Login (request approval)
- [ ] Check email for approval
- [ ] Approve login
- [ ] View dashboard
- [ ] Check payment verifications
- [ ] View transaction IDs and screenshots

### Test Backend API
- [ ] Visit `https://am-fashions-backend.onrender.com/api/health`
- [ ] Should return: `{"status":"OK"}`

---

## 🎯 Final Checklist

- [ ] Backend deployed to Render
- [ ] Database set up (MySQL on Railway/PlanetScale)
- [ ] Main website deployed to Vercel
- [ ] Admin dashboard deployed to Vercel
- [ ] Custom domain connected (yourdomain.com)
- [ ] Admin subdomain connected (admin.yourdomain.com)
- [ ] Environment variables configured
- [ ] CORS updated with production domains
- [ ] Database imported to production
- [ ] All features tested
- [ ] SSL certificates active (automatic)
- [ ] Email service working

---

## 📊 Your Final URLs

```
Main Website:     https://yourdomain.com
Admin Dashboard:  https://admin.yourdomain.com
Backend API:      https://am-fashions-backend.onrender.com
Database:         Railway/PlanetScale MySQL
```

---

## 🔧 Troubleshooting

### Backend not connecting to database
- Check environment variables in Render
- Verify database credentials
- Check database allows external connections

### CORS errors
- Update allowed origins in backend
- Redeploy backend after changes

### Email not sending
- Verify Gmail app password
- Check Render logs for errors

### Domain not working
- Wait for DNS propagation (up to 48 hours)
- Verify DNS records are correct
- Check domain registrar settings

---

## 💰 Cost Breakdown

### Free Tier (Perfect for starting):
- **Vercel**: Free (2 projects)
- **Render**: Free (750 hours/month)
- **Railway MySQL**: $5/month (or use PlanetScale free)
- **MongoDB Atlas**: Free (512MB)
- **Total**: $0-5/month

### Paid Tier (For scaling):
- **Vercel Pro**: $20/month
- **Render Starter**: $7/month
- **Railway Pro**: $20/month
- **Total**: $47/month

---

## 🎉 You're Done!

Your application is now live on:
- ✅ Your custom domain
- ✅ Professional hosting
- ✅ Scalable infrastructure
- ✅ SSL secured
- ✅ Production ready

**Congratulations! 🚀**

---

## 📞 Need Help?

- **Vercel Docs**: https://vercel.com/docs
- **Render Docs**: https://render.com/docs
- **MongoDB Atlas Docs**: https://docs.atlas.mongodb.com

**Contact**: madasumiteesh@gmail.com
