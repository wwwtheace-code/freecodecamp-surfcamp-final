## Free Code Camp Tutorial

You can find all the notes. [here](https://github.com/PaulBratslavsky/freecodecamp-surfcamp-final/tree/main/_resources/notes) 

[Figma Design File Here](https://www.figma.com/design/N27pbzZuIRUm68cjBKuFxv/Surf-Camp-%2F-Sharefile)

# Deployment Guide: Strapi 5 + Next.js 15 (Monorepo) on Render

This guide documents the exact steps taken to deploy this project (Strapi backend and Next.js frontend) to [Render](https://render.com), using a monorepo structure.

---

## 1. Fork the Repository

- Fork this repository to your own GitHub account.

---

## 2. Create a Render Account

- Sign up at [https://render.com](https://render.com) and connect your GitHub account.

---

## 3. Create a PostgreSQL Database on Render

1. In the Render dashboard, click **New +** → **PostgreSQL**.
2. Name your database (e.g. `surfcamp-db`).
3. Choose the **Free** plan and create the database.
4. Once available, copy the **Internal Database URL** (you’ll need this for Strapi).

---

## 4. Deploy the Strapi Backend

1. In Render, click **New +** → **Web Service**.
2. Select **Build and deploy from a Git repository** and choose your forked repo.
3. **Set Root Directory:**  
   - Enter `server` (so Render builds from the Strapi backend folder).
4. **Build Command:**
5. **Start Command:**

6. **Instance Type:** Free
7. **Set Environment Variables:**

| Key                  | Value (example)                                                                 |
|----------------------|---------------------------------------------------------------------------------|
| HOST                 | 0.0.0.0                                                                         |
| PORT                 | 10000                                                                           |
| NODE_ENV             | production                                                                      |
| APP_KEYS             | (random string or use Render's "Generate")                                      |
| API_TOKEN_SALT       | (random string or use Render's "Generate")                                      |
| ADMIN_JWT_SECRET     | (random string or use Render's "Generate")                                      |
| JWT_SECRET           | (random string or use Render's "Generate")                                      |
| DATABASE_URL         | (your Internal Database URL from step 3)                                        |
| TRANSFER_TOKEN_SALT  | (random string, optional but recommended to suppress warnings)                  |

8. **Set Node.js Version:**  
- In the `server` folder, create a file named `.nvmrc` with the contents:
  ```
  20
  ```
- Commit and push to GitHub.

9. **Deploy the service.**  
- Wait for the build to complete and the service to go live.

---

## 5. Set Up Strapi Admin and Permissions

1. Visit your Strapi admin panel:  
`https://<your-strapi-service>.onrender.com/admin`
2. Create your admin user.
3. In Strapi, go to **Settings → Users & Permissions Plugin → Roles → Public**.
4. Enable **find** and **findOne** permissions for all content types your frontend will access (e.g. `global`, `homepage`, `articles`, etc).
5. **Create and publish at least one entry** for each required content type (especially "Global").

---

## 6. Deploy the Next.js Frontend

1. In Render, click **New +** → **Web Service**.
2. Select your forked repo.
3. **Set Root Directory:**  
- Enter `client` (so Render builds from the Next.js frontend folder).
4. **Build Command:**
5. **Start Command:**
6. **Instance Type:** Free
7. **Set Node.js Version:**  
- In the `client` folder, create a file named `.nvmrc` with the contents:
  ```
  20
  ```
- Commit and push to GitHub.
8. **Set Environment Variables:**

| Key              | Value (example)                                         |
|------------------|--------------------------------------------------------|
| STRAPI_API_URL   | https://<your-strapi-service>.onrender.com             |

9. **Deploy the service.**

---

## 7. Fix Next.js Dynamic Data Fetching

- For every page in `client/src/app/` (and subfolders) that fetches data from Strapi, add at the very top:
```js
export const dynamic = "force-dynamic";


