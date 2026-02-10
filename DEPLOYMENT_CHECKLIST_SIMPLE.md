# ✅ Deployment Checklist

Print this and check off as you go!

---

## 🗄️ Database Setup

- [ ] Created Railway account
- [ ] Provisioned MySQL database
- [ ] Copied all 5 connection variables (HOST, USER, PASSWORD, DATABASE, PORT)
- [ ] Imported database schema and data

---

## 🔧 Backend Deployment (Render)

- [ ] Created Render account
- [ ] Created new Web Service
- [ ] Connected GitHub repository
- [ ] Set Root Directory: `admin-dashboard/server`
- [ ] Set Build Command: `npm install`
- [ ] Set Start Command: `npm start`
- [ ] Added all environment variables (11 total):
  - [ ] DB_HOST
  - [ ] DB_USER
  - [ ] DB_PASSWORD
  - [ ] DB_NAME
  - [ ] DB_PORT
  - [ ] PORT
  - [ ] NODE_ENV
  - [ ] JWT_SECRET
  - [ ] EMAIL_USER
  - [ ] EMAIL_PASSWORD
  - [ ] ADMIN_EMAIL
- [ ] Deployment successful
- [ ] Copied backend URL: `_______________________________`
- [ ] Tested health endpoint: `/api/health` returns OK

---

## 🌐 Main Website (Vercel)

- [ ] Updated `.env.production` with backend URL
- [ ] Committed and pushed changes to GitHub
- [ ] Created Vercel account
- [ ] Imported GitHub repository
- [ ] Set Framework: Create React App
- [ ] Set Root Directory: `./` (blank)
- [ ] Set Build Command: `npm run build`
- [ ] Set Output Directory: `build`
- [ ] Added environment variable: `REACT_APP_API_URL`
- [ ] Deployment successful
- [ ] Website URL: `_______________________________`
- [ ] Tested website - can browse products
- [ ] Tested cart functionality
- [ ] Tested order placement

---

## 👨‍💼 Admin Dashboard (Vercel)

- [ ] Updated `admin-dashboard/client/.env.production` with backend URL
- [ ] Committed and pushed changes to GitHub
- [ ] Created new Vercel project (same repo)
- [ ] Set Framework: Vite
- [ ] Set Root Directory: `admin-dashboard/client`
- [ ] Set Build Command: `npm run build`
- [ ] Set Output Directory: `dist`
- [ ] Added environment variable: `VITE_API_URL`
- [ ] Deployment successful
- [ ] Admin URL: `_______________________________`
- [ ] Tested login flow
- [ ] Tested email approval system
- [ ] Can view dashboard after approval

---

## 🔄 Post-Deployment

- [ ] Updated CORS in Render with actual Vercel URLs
- [ ] Backend redeployed with new CORS settings
- [ ] Tested website → backend communication (no CORS errors)
- [ ] Tested admin → backend communication (no CORS errors)
- [ ] Email notifications working
- [ ] Payment screenshot upload working
- [ ] All features tested end-to-end

---

## 🌍 Custom Domain (Optional)

- [ ] Added custom domain in Vercel (main website)
- [ ] Added DNS records at domain registrar
- [ ] Domain verified and SSL active
- [ ] Added admin subdomain in Vercel
- [ ] Admin subdomain DNS configured
- [ ] Updated CORS with custom domains
- [ ] Main domain: `_______________________________`
- [ ] Admin domain: `_______________________________`

---

## 📊 Final URLs

Write your deployed URLs here:

```
Main Website:     https://________________________________
Admin Dashboard:  https://________________________________
Backend API:      https://________________________________
Database:         Railway MySQL
```

---

## 🎉 Success Criteria

All these should work:

- [ ] Customer can visit website and see products
- [ ] Customer can add items to cart
- [ ] Customer can place order
- [ ] Customer can upload payment screenshot
- [ ] Customer receives order confirmation email
- [ ] Admin receives new order notification email
- [ ] Admin can request login
- [ ] Admin receives approval email
- [ ] Admin can approve their own login
- [ ] Admin can view dashboard
- [ ] Admin can see payment verifications
- [ ] Admin can view transaction IDs and screenshots
- [ ] No console errors in browser
- [ ] No CORS errors
- [ ] Backend health check returns OK

---

## 📝 Important Info to Save

**Railway MySQL:**
```
Host: _______________________________
User: _______________________________
Password: _______________________________
Database: _______________________________
Port: _______________________________
```

**Render Backend:**
```
URL: _______________________________
JWT Secret: _______________________________
```

**Vercel Projects:**
```
Main Website: _______________________________
Admin Dashboard: _______________________________
```

---

## 🔧 Troubleshooting

If something doesn't work:

1. **Check Render Logs**:
   - Render Dashboard → Your service → Logs tab
   - Look for errors

2. **Check Vercel Logs**:
   - Vercel Dashboard → Your project → Deployments
   - Click on deployment → View Function Logs

3. **Check Browser Console**:
   - F12 → Console tab
   - Look for errors (especially CORS or network errors)

4. **Test Backend Directly**:
   - Visit: `https://your-backend.onrender.com/api/health`
   - Should return: `{"status":"OK"}`

5. **Verify Environment Variables**:
   - Render: Environment tab
   - Vercel: Settings → Environment Variables
   - Make sure all are set correctly

---

## 💡 Quick Fixes

**Backend won't start:**
→ Check database connection variables
→ Check Render logs for specific error

**CORS errors:**
→ Update FRONTEND_URL and ADMIN_URL in Render
→ Remove trailing slashes from URLs
→ Redeploy backend

**Build fails:**
→ Check build logs
→ Verify all dependencies in package.json
→ Try deploying again

**Database connection fails:**
→ Check Railway MySQL is running
→ Verify all 5 database variables are correct
→ Check database allows external connections

---

## 📞 Support

- Full Guide: `DEPLOYMENT_STEPS.md`
- Quick Start: `QUICK_START_DEPLOYMENT.md`
- Email: madasumiteesh@gmail.com

---

**Date Started: _______________**
**Date Completed: _______________**
**Total Time: _______________**

🎉 **Congratulations on your deployment!** 🎉
