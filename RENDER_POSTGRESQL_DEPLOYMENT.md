# 🆓 100% FREE Deployment - Render PostgreSQL + Vercel

**Total Cost: $0/month** - No credit card required!

---

## 🎯 What You'll Use

1. **Render PostgreSQL** - Database (1GB FREE)
2. **Render Web Service** - Backend API (750 hours/month FREE)
3. **Vercel** - Frontend (Unlimited FREE)

---

## 📝 Quick Start (30 minutes)

### STEP 1: Create Render PostgreSQL Database (5 min)

1. Go to [render.com](https://render.com) → Sign in with GitHub

2. Click **New +** → **PostgreSQL**

3. Configure:
   ```
   Name: am-fashions-db
   Database: amfashions
   User: amfashions
   Region: Oregon (US West) or closest to you
   PostgreSQL Version: 16
   Instance Type: Free
   ```

4. Click **Create Database** → Wait 2-3 minutes

5. **Copy Internal Database URL**:
   - Scroll to **Connections** section
   - Copy **Internal Database URL**
   - Example: `postgres://amfashions:xxxxx@dpg-xxxxx.oregon-postgres.render.com/amfashions`
   - **Save this!** You'll need it

6. **Import Schema**:
   - Click **Shell** tab in database dashboard
   - Paste contents of `admin-dashboard/database/postgresql_setup.sql`
   - Or use psql locally:
     ```bash
     psql <your-database-url> < admin-dashboard/database/postgresql_setup.sql
     ```

---

### STEP 2: Update Code for PostgreSQL (5 min)

#### 2.1 Install PostgreSQL Driver

```bash
cd admin-dashboard/server
npm install pg
npm uninstall mysql2
```

#### 2.2 Update Server Configuration

Edit `admin-dashboard/server/server.js`:

**Find this line (around line 3):**
```javascript
import db from './config/database.js';
```

**Change to:**
```javascript
import db from './config/database-postgres.js';
```

**Find this section (around line 45-52):**
```javascript
const db = mysql.createPool({
  host: process.env.DB_HOST || 'localhost',
  user: process.env.DB_USER || 'root',
  password: process.env.DB_PASSWORD || '',
  database: process.env.DB_NAME || 'admin_dashboard',
  waitForConnections: true,
  connectionLimit: 10,
  queueLimit: 0
});
```

**Replace with:**
```javascript
// Database connection is now handled by database-postgres.js
// Import it at the top of the file
```

**Find this section (around line 60-67):**
```javascript
// Test database connection
db.getConnection((err, connection) => {
  if (err) {
    console.error('❌ Database connection failed:', err.message);
    process.exit(1);
  }
  console.log('✅ Database connected successfully');
  connection.release();
});
```

**Replace with:**
```javascript
// Test database connection
db.testConnection();
```

**Find this line (around line 70):**
```javascript
app.locals.db = dbPromise;
```

**Replace with:**
```javascript
app.locals.db = db;
```

#### 2.3 Update Package.json

Edit `admin-dashboard/server/package.json`:

**Find:**
```json
"dependencies": {
  "bcryptjs": "^2.4.3",
  "cors": "^2.8.6",
  "dotenv": "^16.6.1",
  "express": "^4.22.1",
  "jsonwebtoken": "^9.0.2",
  "multer": "^2.0.2",
  "mysql2": "^3.16.3",
  "nodemailer": "^8.0.1"
}
```

**Change to:**
```json
"dependencies": {
  "bcryptjs": "^2.4.3",
  "cors": "^2.8.6",
  "dotenv": "^16.6.1",
  "express": "^4.22.1",
  "jsonwebtoken": "^9.0.2",
  "multer": "^2.0.2",
  "pg": "^8.11.3",
  "nodemailer": "^8.0.1"
}
```

#### 2.4 Commit Changes

```bash
git add .
git commit -m "Switch to PostgreSQL for Render deployment"
git push origin main
```

---

### STEP 3: Deploy Backend to Render (8 min)

1. **Render Dashboard** → **New +** → **Web Service**

2. **Connect Repository**: `am-fashions/am-with-emailjs`

3. **Configure**:
   ```
   Name: am-fashions-backend
   Region: Same as database (Oregon)
   Branch: main
   Root Directory: admin-dashboard/server
   Runtime: Node
   Build Command: npm install
   Start Command: npm start
   Instance Type: Free
   ```

4. **Environment Variables** (Click Advanced):
   ```
   DATABASE_URL=<paste-your-internal-database-url-from-step-1>
   PORT=5000
   NODE_ENV=production
   JWT_SECRET=<generate-random-string>
   EMAIL_USER=madasumiteesh@gmail.com
   EMAIL_PASSWORD=mnfc xdxe ojpi rtzf
   ADMIN_EMAIL=madasumiteesh@gmail.com
   FRONTEND_URL=https://yourdomain.com
   ADMIN_URL=https://admin.yourdomain.com
   ```

   **Generate JWT_SECRET:**
   ```bash
   node -e "console.log(require('crypto').randomBytes(32).toString('hex'))"
   ```

5. **Create Web Service** → Wait 5-10 minutes

6. **Copy Backend URL**: `https://am-fashions-backend.onrender.com`

7. **Test**: Visit `https://am-fashions-backend.onrender.com/api/health`

---

### STEP 4: Deploy Main Website to Vercel (5 min)

1. **Update `.env.production`**:
   ```env
   REACT_APP_API_URL=https://am-fashions-backend.onrender.com/api
   ```

2. **Commit**:
   ```bash
   git add .
   git commit -m "Update production API URL"
   git push origin main
   ```

3. **Vercel Dashboard** → **New Project**

4. **Import**: `am-fashions/am-with-emailjs`

5. **Configure**:
   ```
   Framework: Create React App
   Root Directory: ./
   Build Command: npm run build
   Output Directory: build
   ```

6. **Environment Variables**:
   ```
   REACT_APP_API_URL=https://am-fashions-backend.onrender.com/api
   ```

7. **Deploy** → Copy URL

---

### STEP 5: Deploy Admin Dashboard to Vercel (5 min)

1. **Update `admin-dashboard/client/.env.production`**:
   ```env
   VITE_API_URL=https://am-fashions-backend.onrender.com/api
   ```

2. **Commit**:
   ```bash
   git add .
   git commit -m "Update admin API URL"
   git push origin main
   ```

3. **Vercel Dashboard** → **New Project**

4. **Import**: Same repo `am-fashions/am-with-emailjs`

5. **Configure**:
   ```
   Framework: Vite
   Root Directory: admin-dashboard/client
   Build Command: npm run build
   Output Directory: dist
   ```

6. **Environment Variables**:
   ```
   VITE_API_URL=https://am-fashions-backend.onrender.com/api
   ```

7. **Deploy** → Copy URL

---

### STEP 6: Update CORS (2 min)

1. **Render** → Your backend service → **Environment**

2. **Update**:
   ```
   FRONTEND_URL=https://your-website.vercel.app
   ADMIN_URL=https://your-admin.vercel.app
   ```

3. **Save** (auto-redeploys)

---

## ✅ Test Everything

### Backend
Visit: `https://your-backend.onrender.com/api/health`
Should see: `{"status":"OK"}`

### Website
1. Browse products ✅
2. Add to cart ✅
3. Place order ✅
4. Upload payment screenshot ✅

### Admin
1. Request login ✅
2. Check email ✅
3. Approve login ✅
4. View dashboard ✅

---

## 🎉 You're Live!

```
Website:  https://your-site.vercel.app
Admin:    https://your-admin.vercel.app
API:      https://your-backend.onrender.com
Database: Render PostgreSQL (1GB FREE)

Total Cost: $0/month
```

---

## 💡 Important Notes

### Render Free Tier
- ✅ 1GB PostgreSQL database
- ✅ 750 hours/month backend (more than enough)
- ⚠️ Backend sleeps after 15 min inactivity
- ⚠️ First request takes 30-60 seconds to wake

### Keep Backend Awake (Optional)
Use [UptimeRobot](https://uptimerobot.com) (free):
1. Add monitor: `https://your-backend.onrender.com/api/health`
2. Check every 5 minutes
3. Backend stays awake!

### Vercel Free Tier
- ✅ Unlimited bandwidth
- ✅ Automatic SSL
- ✅ Global CDN
- ✅ Auto-deploy on git push

---

## 🔧 Troubleshooting

**"Cannot connect to database"**
→ Check DATABASE_URL in Render environment variables
→ Make sure you used Internal Database URL (not External)

**"Backend returns 404"**
→ Check Root Directory is `admin-dashboard/server`
→ Verify Start Command is `npm start`

**CORS errors**
→ Update FRONTEND_URL and ADMIN_URL in Render
→ Make sure URLs match exactly (no trailing slash)

**Build fails**
→ Check Render logs for specific error
→ Verify pg package is in package.json
→ Make sure you committed all changes

---

## 📊 Free Tier Limits

### Render PostgreSQL
- 1GB storage (enough for thousands of products/orders)
- 97 hours/month uptime (database stays on)
- Shared CPU

### Render Web Service
- 750 hours/month (31 days!)
- 512MB RAM
- Sleeps after 15 min inactivity

### Vercel
- 100GB bandwidth/month
- Unlimited deployments
- Unlimited projects

**Perfect for starting out!**

---

## 🚀 Auto-Deploy

Push to GitHub = Auto-deploy everywhere:

```bash
git add .
git commit -m "Update feature"
git push origin main
```

Both Render and Vercel watch your repo!

---

## 📞 Support

- **Render Docs**: https://render.com/docs
- **Vercel Docs**: https://vercel.com/docs
- **PostgreSQL Docs**: https://www.postgresql.org/docs/

**Questions?** madasumiteesh@gmail.com

---

## 🎊 Congratulations!

You've deployed a full-stack e-commerce platform with:
- ✅ PostgreSQL database
- ✅ REST API backend
- ✅ React frontend
- ✅ Admin dashboard
- ✅ Payment verification
- ✅ Email notifications
- ✅ SSL certificates
- ✅ Auto-deploy

**All for $0/month!** 🎉
