# 🚀 Complete Inventory App Launch Guide (Full Stack + Hosting)

This guide walks you through building, securing, and launching the full Inventory App stack with:

- React frontend (Auth0 + secure API calls)
- Express backend (SOC 2–aligned + Algolia integration)
- GitHub CI/CD via GitHub Actions
- Vercel frontend hosting
- Railway backend Docker hosting
- Secure roles, settings, search, and editing

---

## ✅ Step 1: Setup Accounts

### 1. Auth0 (https://auth0.com)
- Create account → Applications → “Single Page App”
- Get:
  - `Domain`
  - `Client ID`
- Go to Roles tab → create:
  - `admin`, `can_edit`, `can_view`
- Optional: Add Post Login Action to assign roles

### 2. Algolia (https://algolia.com)
- Sign up → Create app → create index `inventory`
- Save:
  - `Application ID`
  - `Admin API Key`
  - `Search-Only API Key`

### 3. Vercel (https://vercel.com)
- Connect GitHub repo
- Add ENV vars in frontend project:
  - `REACT_APP_AUTH0_DOMAIN`
  - `REACT_APP_AUTH0_CLIENT_ID`
  - `REACT_APP_BACKEND_URL=https://<your-backend>.railway.app`

### 4. Railway (https://railway.app)
- Create project → link GitHub → select backend folder
- In settings, add `.env` values:
  - `PORT=4000`
  - `AUTH0_AUDIENCE=https://inventory-api`
  - `AUTH0_DOMAIN=<your-auth0-domain>`
  - `ALGOLIA_APP_ID`
  - `ALGOLIA_ADMIN_KEY`
  - `ALGOLIA_INDEX_NAME=inventory`

---

## ✅ Step 2: Repo Setup

Folder structure:
```
inventory-app/
├── frontend/        # React App
│   ├── .env         # Auth0 and backend env
│   ├── Dockerfile
├── backend/         # Express API
│   ├── .env         # Algolia and Auth0 env
│   ├── Dockerfile
│   ├── index.js
│   ├── routes/
│   │   └── inventory.js
│   └── services/
│       └── algolia.js
├── docker-compose.yml
├── vercel.json
├── .github/
│   └── workflows/
│       └── deploy.yml
```

---

## ✅ Step 3: Local Dev (Optional)

```bash
docker-compose up --build
```

- Frontend: http://localhost:3000
- Backend: http://localhost:4000

---

## ✅ Step 4: GitHub + CI/CD

1. Push to GitHub:
```bash
git init
git remote add origin https://github.com/YOUR_NAME/inventory-app.git
git add .
git commit -m "Initial commit"
git push -u origin main
```

2. Go to GitHub → Settings → Secrets → Add:

| Secret Name               | Value                        |
|---------------------------|------------------------------|
| `VERCEL_TOKEN`            | From Vercel account          |
| `DOCKER_USERNAME`         | *(optional)* DockerHub       |
| `DOCKER_PASSWORD`         | *(optional)* DockerHub token |
| `AUTH0_AUDIENCE`          | https://inventory-api        |
| `AUTH0_DOMAIN`            | Auth0 domain                 |
| `ALGOLIA_APP_ID`          | From Algolia                 |
| `ALGOLIA_ADMIN_KEY`       | From Algolia                 |
| `ALGOLIA_INDEX_NAME`      | inventory                    |
| `REACT_APP_BACKEND_URL`   | From Railway                 |

---

## ✅ Step 5: Deploy Frontend to Vercel

- Vercel auto-deploys on push
- Uses `vercel.json` for SPA routing + ENV injection

---

## ✅ Step 6: Deploy Backend to Railway

- Railway builds from `backend/`
- Auto HTTPS, Docker-native, logs, and scaling
- Use URL like: `https://secure-api.up.railway.app`

---

## ✅ Step 7: Test + Monitor

- Log in with Auth0
- Use admin role to add/edit/delete
- Inventory flows via secure API to Algolia

---

🎉 You're done! SOC 2–aligned inventory search app, deployed with modern tools.
