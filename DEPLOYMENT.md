# Deployment Guide

## Deploy to Render.com (Free Tier)

Render.com provides free hosting with WebSocket support, perfect for these CRDT demos.

### Prerequisites
- GitHub account (you already have this)
- Render.com account (sign up at https://render.com with your GitHub account)

### Deployment Steps

#### 1. Sign Up for Render
1. Go to https://render.com
2. Click "Get Started for Free"
3. Sign up with your GitHub account
4. Authorize Render to access your repositories

#### 2. Deploy Text Editor Demo

1. Go to https://dashboard.render.com/
2. Click "New +" → "Web Service"
3. Connect your GitHub repository: `Abu-thahir/RGA-CRDT-IMPLEMENTATION-DEMO`
4. Configure the service:
   - **Name**: `crdt-text-editor` (or any name you prefer)
   - **Root Directory**: `RGA-CRDT-IMPLEMENTATION-DEMO/crdt-web`
   - **Runtime**: Node
   - **Build Command**: `npm install && npm run build`
   - **Start Command**: `npm run dev`
   - **Instance Type**: Free
5. Click "Create Web Service"
6. Wait 2-3 minutes for deployment

#### 3. Deploy Block Editor Demo

1. Click "New +" → "Web Service" again
2. Connect the same repository
3. Configure the service:
   - **Name**: `crdt-block-editor` (or any name you prefer)
   - **Root Directory**: `RGA-CRDT-IMPLEMENTATION-DEMO/crdt-block-web-ui`
   - **Runtime**: Node
   - **Build Command**: `npm install && npm run build`
   - **Start Command**: `npm run dev`
   - **Instance Type**: Free
4. Click "Create Web Service"
5. Wait 2-3 minutes for deployment

### After Deployment

You'll get URLs like:
- Text Editor: `https://crdt-text-editor.onrender.com`
- Block Editor: `https://crdt-block-editor.onrender.com`

**Note**: Free tier services spin down after 15 minutes of inactivity. The first request after inactivity may take 30-60 seconds to wake up.

### Testing Your Deployed Demos

1. Open the text editor URL in multiple browser tabs
2. Type in one tab and watch it sync to others
3. Same for the block editor!

### Alternative: Railway.app

If you prefer Railway:

1. Go to https://railway.app
2. Sign up with GitHub
3. Click "New Project" → "Deploy from GitHub repo"
4. Select your repository
5. Add two services (one for each demo)
6. Configure root directory and start commands as above

### Alternative: Fly.io

If you prefer Fly.io:

1. Install Fly CLI: `curl -L https://fly.io/install.sh | sh`
2. Sign up: `fly auth signup`
3. Deploy text editor:
   ```bash
   cd RGA-CRDT-IMPLEMENTATION-DEMO/crdt-web
   fly launch
   ```
4. Deploy block editor:
   ```bash
   cd RGA-CRDT-IMPLEMENTATION-DEMO/crdt-block-web-ui
   fly launch
   ```

## Troubleshooting

### Build Fails
- Check that Node version is 20+
- Verify the root directory is set correctly
- Check build logs for specific errors

### WebSocket Connection Fails
- Ensure the service is using HTTPS (Render does this automatically)
- Check browser console for connection errors
- Verify the WebSocket URL in client code

### Service Won't Start
- Check that `npm run dev` works locally
- Verify all dependencies are in package.json
- Check service logs in Render dashboard

## Cost

All three platforms offer free tiers:
- **Render**: 750 hours/month free (enough for 1-2 services running 24/7)
- **Railway**: $5 free credit/month
- **Fly.io**: 3 shared VMs free

For these demos, Render's free tier is recommended.
