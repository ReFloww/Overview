# Backend Deployment

Deploy the NestJS backend to production.

## Railway

### 1. Create Project

1. Go to [railway.app](https://railway.app)
2. Create new project
3. Connect GitHub repository

### 2. Add PostgreSQL

1. Click "New" → "Database" → "PostgreSQL"
2. Copy connection string

### 3. Configure Environment

Add variables in Railway dashboard:

```
DATABASE_URL=<railway postgres url>
PORT=3000
NODE_ENV=production
JWT_SECRET=<secure-secret>
JWT_REFRESH_SECRET=<secure-refresh-secret>
RPC_URL=https://rpc.mantle.xyz
CHAIN_ID=5000
```

### 4. Deploy

Railway auto-deploys on push to main branch.

## Render

### 1. Create Web Service

1. Go to [render.com](https://render.com)
2. New → Web Service
3. Connect repository

### 2. Configure

- Build Command: `npm install && npx prisma generate && npm run build`
- Start Command: `npm run start:prod`

### 3. Add Database

Create PostgreSQL database in Render.

### 4. Environment Variables

Add in Render dashboard.

## Manual Deployment

### Build

```bash
cd Backend
npm install
npx prisma generate
npm run build
```

### Run

```bash
npm run start:prod
```

### With PM2

```bash
npm install -g pm2
pm2 start dist/main.js --name reflow-backend
pm2 save
```

## Database Migration

Run on first deployment:

```bash
npx prisma migrate deploy
```

## Health Check

Endpoint: `GET /health`

Expected response:
```json
{ "status": "ok" }
```
