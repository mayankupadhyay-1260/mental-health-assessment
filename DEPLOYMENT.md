# Deployment Guide: Mental Health Assessment Platform

This guide provides step-by-step instructions to deploy the whole MERN stack project so that anyone on the internet can access and use it.

We'll deploy this entire project as a **Single Full-Stack Application** on Render. The backend will seamlessly serve the static frontend React files, so everything lives under one URL!

- **MongoDB Atlas** for the database
- **Render** for the unified Frontend & Backend
---

## 1. Prepare Your Database (MongoDB Atlas)
If you are currently using a local database with **MongoDB Compass**, you must migrate your data to **MongoDB Atlas** so your deployed backend can reach it over the internet.
1. Go to your [MongoDB Atlas Dashboard](https://cloud.mongodb.com/) and create a free cluster.
2. Under **Security** in the left sidebar, click on **Network Access** and add **Allow Access From Anywhere** (`0.0.0.0/0`).
3. Go to **Database** -> **Connect** -> **Drivers** and copy your Connection String (e.g., `mongodb+srv://<username>:<password>@cluster.mongodb.net/database_name`).
4. **Migrate existing local data:** Paste this Connection String into your local **MongoDB Compass** application to connect to your live Atlas database. You can then use Compass to import your local JSON data collections directly to the cloud!
   - *Keep this Connection String handy for the Backend deployment.*

---

## 2. Deploy Everything (Render)
Render makes it very easy to deploy Node.js apps. We have set up the project so Render will automatically build the frontend and backend together!

1. Create an account on [Render](https://render.com/).
2. From your Render Dashboard, click **New +** and select **Web Service**.
3. Connect your GitHub account and select your repository (`khanndelwalharshit/MENTAL-HEALTH-ASSESSMENT`).
4. **Configure the Service:**
   - **Name:** `mental-health-app`
   - **Root Directory:** *(leave this completely blank!)*
   - **Environment:** `Node`
   - **Build Command:** `npm run build`
   - **Start Command:** `npm start`
   - **Instance Type:** `Free`
5. **Add Environment Variables:**
   Scroll down to the "Environment Variables" section and add:
   - `NODE_ENV`: `production` (⚠️ **Crucial**: This tells the backend to serve the frontend files!)
   - `MONGO_URI`: *Your MongoDB connection string from Step 1*
   - `JWT_SECRET`: *Your secret key (e.g., `superadvancedjwtkeyforauth...`)*
   - `GOOGLE_CLIENT_ID`: *Your Google Client ID* (⚠️ **Crucial:** Must be set here since it is not hardcoded in the codebase)
   - `GOOGLE_CLIENT_SECRET`: *Your Google Client Secret* (⚠️ **Crucial:** Must be set here!)
   - `FRONTEND_URL`: *The URL Render gives you (e.g. `https://mental-health-app.onrender.com`)*
6. Click **Create Web Service**.
7. Render will now do the heavy lifting: it will install backend dependencies, install frontend dependencies, build the React app, and start the unified server. Wait for it to finish and show "Live".

---

## 3. Google OAuth Config
If using Google Login, go to your Google Cloud Console and update the **Authorized JavaScript origins** and **Authorized redirect URIs** to match your deployed Render URL.

Enjoy your live application! 🎉 Anyone with the Render link can now access your entire full-stack platform.
