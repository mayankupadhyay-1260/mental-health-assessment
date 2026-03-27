# Deployment Guide: Mental Health Assessment Platform

This guide provides step-by-step instructions to deploy the whole MERN stack project so that anyone on the internet can access and use it.

We'll use free-tier friendly platforms:
- **MongoDB Atlas** for the database
- **Render** for the Node.js/Express Backend
- **Vercel** for the React (Vite) Frontend

---

## 1. Prepare Your Database (MongoDB Atlas)
If you are currently using a local database with **MongoDB Compass**, you must migrate your data to **MongoDB Atlas** so your deployed backend can reach it over the internet.
1. Go to your [MongoDB Atlas Dashboard](https://cloud.mongodb.com/) and create a free cluster.
2. Under **Security** in the left sidebar, click on **Network Access** and add **Allow Access From Anywhere** (`0.0.0.0/0`).
3. Go to **Database** -> **Connect** -> **Drivers** and copy your Connection String (e.g., `mongodb+srv://<username>:<password>@cluster.mongodb.net/database_name`).
4. **Migrate existing local data:** Paste this Connection String into your local **MongoDB Compass** application to connect to your live Atlas database. You can then use Compass to import your local JSON data collections directly to the cloud!
   - *Keep this Connection String handy for the Backend deployment.*

---

## 2. Deploy the Backend (Render)
Render is a cloud platform that makes it very easy to deploy Node.js apps.

1. Create an account on [Render](https://render.com/).
2. From your Render Dashboard, click **New +** and select **Web Service**.
3. Connect your GitHub account and select your repository (`khanndelwalharshit/MENTAL-HEALTH-ASSESSMENT`).
4. **Configure the Service:**
   - **Name:** `mental-health-backend` (or similar)
   - **Root Directory:** `backend` (⚠️ **Important:** Ensure this is exactly `backend`)
   - **Environment:** `Node`
   - **Build Command:** `npm install`
   - **Start Command:** `npm start`
   - **Instance Type:** `Free`
5. **Add Environment Variables:**
   Scroll down to the "Environment Variables" section and add:
   - `MONGO_URI`: *Your MongoDB connection string from Step 1*
   - `PORT`: `5000`
   - `JWT_SECRET`: *Your secret key (e.g., `superadvancedjwtkeyforauth...`)*
   - `GOOGLE_CLIENT_ID`: *Your Google Client ID* (⚠️ **Crucial:** Must be set here since it is not hardcoded in the codebase due to GitHub Push Protection)
   - `GOOGLE_CLIENT_SECRET`: *Your Google Client Secret* (⚠️ **Crucial:** Must be set here!)
   - `FRONTEND_URL`: *You will update this later once the frontend is deployed.*
6. Click **Create Web Service**.
7. Render will now build and deploy the backend. Wait for it to finish and show "Live".
8. Copy the backend URL (e.g., `https://mental-health-backend.onrender.com`). You will need this for the frontend!

---

## 3. Deploy the Frontend (Vercel)
Vercel is optimized for building and deploying React applications with Vite.

1. **API URLs in Frontend Code:**
   Your React app is already configured to automatically use the deployed backend URL in production! This was set globally in `frontend/src/main.jsx`:
   ```javascript
   axios.defaults.baseURL = import.meta.env.VITE_API_URL || 'https://mental-health-assessment-kjny.onrender.com';
   ```
2. Make sure all your changes are pushed to GitHub.
3. Go to [Vercel.com](https://vercel.com/) and sign up with GitHub.
4. Click **Add New** -> **Project**.
5. Import your `MENTAL-HEALTH-ASSESSMENT` repository.
6. **Configure the Project:**
   - **Framework Preset:** `Vite`
   - **Root Directory:** Edit this and select `frontend` (⚠️ **Important!**)
7. **Environment Variables:**
   If your frontend uses any `.env` variables (like `VITE_API_URL`), add them here.
   - Example: `VITE_API_URL` = `https://mental-health-backend.onrender.com`
8. Click **Deploy**.
9. Vercel will build your React application and give you a live public URL (e.g., `https://mental-health-assessment.vercel.app`).

---

## 4. Final Connections
Now that both are deployed, make sure they can talk to each other:

1. **CORS:** Ensure your backend's `cors` configuration allows requests from your new Vercel frontend URL. Open `backend/server.js` or `backend/app.js` and verify CORS:
   ```javascript
   app.use(cors({
     origin: 'https://your-frontend-domain.vercel.app', // Update this!
     credentials: true
   }));
   ```
   *If you change this, push the code to GitHub so Render redeploys.*
2. **Google OAuth Config:** If using Google Login, go to your Google Cloud Console and update the **Authorized JavaScript origins** and **Authorized redirect URIs** to match your deployed frontend and backend URLs.
3. Enjoy your live application! 🎉 Anyone with the Vercel link can now access it.
