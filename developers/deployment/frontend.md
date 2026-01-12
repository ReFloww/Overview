# Frontend Deployment

Deploy the Next.js frontend to production.

## Vercel (Recommended)

### 1. Connect Repository

1. Go to [vercel.com](https://vercel.com)
2. Import Git repository
3. Select the Frontend folder

### 2. Configure Build

- Framework Preset: Next.js
- Root Directory: `Frontend`
- Build Command: `npm run build`
- Output Directory: `.next`

### 3. Environment Variables

Add in Vercel dashboard:

```
NEXT_PUBLIC_API_BASE_URL=https://api.reflow.io/v1
NEXT_PUBLIC_WALLETCONNECT_PROJECT_ID=<your-id>
NEXT_PUBLIC_USDT_ADDRESS=0x...
NEXT_PUBLIC_FACTORY_ADDRESS=0x...
NEXT_PUBLIC_CHAIN_ID=5000
```

### 4. Deploy

Vercel auto-deploys on push.

## Netlify

### 1. Create Site

1. Go to [netlify.com](https://netlify.com)
2. New site from Git
3. Connect repository

### 2. Build Settings

- Base directory: `Frontend`
- Build command: `npm run build`
- Publish directory: `Frontend/.next`

### 3. Environment Variables

Add in Netlify dashboard.

## Manual Build

```bash
cd Frontend
npm install
npm run build
```

### Static Export

If needed:
```bash
npm run build
npm run export
```

Serve the `out` directory.

## Configuration

### next.config.js

```javascript
module.exports = {
  output: 'standalone',
  // For static hosting:
  // output: 'export',
};
```

## Post-Deployment

1. Verify all pages load
2. Test wallet connection
3. Test API integration
4. Check console for errors
