# Deployment Guide

This guide covers multiple ways to deploy the Paxos Visualizer application.

## Quick Start with Docker Compose

The easiest way to deploy locally or on a server:

```bash
# Build and start both services
docker-compose up -d

# View logs
docker-compose logs -f

# Stop services
docker-compose down
```

The application will be available at:
- Frontend: http://localhost
- Backend API: http://localhost:8000
- API Docs: http://localhost:8000/docs

## Platform-Specific Deployment

### Option 1: Render.com (Recommended for Free Tier)

#### Backend Deployment:
1. Go to [Render.com](https://render.com) and create an account
2. Click "New +" → "Web Service"
3. Connect your GitHub repository
4. Configure:
   - **Name**: `paxos-backend`
   - **Environment**: `Docker`
   - **Dockerfile Path**: `Dockerfile`
   - **Root Directory**: `/` (root of repo)
   - **Port**: `8000`
5. Add environment variable: `PYTHONUNBUFFERED=1`
6. Deploy!

#### Frontend Deployment:
1. Click "New +" → "Static Site"
2. Connect your GitHub repository
3. Configure:
   - **Name**: `paxos-frontend`
   - **Root Directory**: `paxos-visualizer-ui`
   - **Build Command**: `npm install && npm run build`
   - **Publish Directory**: `build`
4. Add environment variable: `REACT_APP_API_URL=https://your-backend-url.onrender.com`
5. Deploy!

### Option 2: Railway.app

1. Go to [Railway.app](https://railway.app)
2. Create a new project
3. Add two services:
   - **Backend**: Connect Dockerfile from root
   - **Frontend**: Connect Dockerfile from `paxos-visualizer-ui/`
4. Set environment variable for frontend: `REACT_APP_API_URL=<backend-url>`
5. Deploy!

### Option 3: DigitalOcean App Platform

1. Go to [DigitalOcean App Platform](https://www.digitalocean.com/products/app-platform)
2. Create a new app from GitHub
3. Add two components:
   - **Backend Component**:
     - Type: Web Service
     - Source: Root directory
     - Dockerfile: `Dockerfile`
     - Port: 8000
   - **Frontend Component**:
     - Type: Static Site
     - Source: `paxos-visualizer-ui/`
     - Build Command: `npm install && npm run build`
     - Output Directory: `build`
4. Set environment variable: `REACT_APP_API_URL=<backend-url>`
5. Deploy!

### Option 4: AWS (EC2/ECS)

#### Using EC2:
1. Launch an EC2 instance (Ubuntu 22.04)
2. Install Docker and Docker Compose:
   ```bash
   sudo apt update
   sudo apt install docker.io docker-compose -y
   sudo usermod -aG docker $USER
   ```
3. Clone your repository
4. Run `docker-compose up -d`
5. Configure security groups to allow ports 80 and 8000

#### Using ECS:
1. Push Docker images to ECR
2. Create ECS cluster and services
3. Configure load balancer
4. Set environment variables

### Option 5: Heroku

#### Backend:
1. Install Heroku CLI
2. Create `Procfile` in root:
   ```
   web: uvicorn app.main:app --host 0.0.0.0 --port $PORT
   ```
3. Deploy:
   ```bash
   heroku create paxos-backend
   git push heroku main
   ```

#### Frontend:
1. Use [Heroku Buildpack for React](https://github.com/mars/create-react-app-buildpack)
2. Set environment variable: `REACT_APP_API_URL=<backend-url>`

## Environment Variables

### Backend
- `PYTHONUNBUFFERED=1` (recommended for logging)

### Frontend
- `REACT_APP_API_URL` - Backend API URL (required for production)
  - Development: `http://localhost:8000`
  - Production: `https://your-backend-domain.com`

## Manual Deployment (VPS/Server)

1. **Install dependencies on server:**
   ```bash
   sudo apt update
   sudo apt install docker.io docker-compose nginx -y
   ```

2. **Clone repository:**
   ```bash
   git clone <your-repo-url>
   cd PaxosVisualizer
   ```

3. **Update frontend API URL:**
   Edit `paxos-visualizer-ui/src/constants.js` or set environment variable

4. **Build and run:**
   ```bash
   docker-compose up -d --build
   ```

5. **Configure Nginx (optional, for custom domain):**
   ```nginx
   server {
       listen 80;
       server_name your-domain.com;
       
       location / {
           proxy_pass http://localhost:80;
       }
       
       location /api {
           proxy_pass http://localhost:8000;
       }
   }
   ```

## Troubleshooting

### Backend not responding
- Check if port 8000 is accessible
- Verify CORS settings in `app/main.py`
- Check logs: `docker-compose logs backend`

### Frontend can't connect to backend
- Verify `REACT_APP_API_URL` is set correctly
- Check CORS configuration
- Ensure backend is running and accessible

### Build failures
- Ensure all dependencies are in `requirements.txt`
- Check Node.js version (should be 18+)
- Verify Dockerfile syntax

## Production Checklist

- [ ] Set `REACT_APP_API_URL` to production backend URL
- [ ] Configure CORS to allow only your frontend domain
- [ ] Set up SSL/HTTPS (Let's Encrypt recommended)
- [ ] Configure proper logging
- [ ] Set up monitoring/health checks
- [ ] Configure backup strategy
- [ ] Set up CI/CD pipeline
- [ ] Review security settings

## Support

For issues, check:
- Backend logs: `docker-compose logs backend`
- Frontend logs: `docker-compose logs frontend`
- API docs: `http://your-backend-url/docs`

