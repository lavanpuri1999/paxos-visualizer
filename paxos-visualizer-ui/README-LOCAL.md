# Running UI Locally for Debugging

## Quick Start

1. **Set your backend URL** in `.env.local`:
   ```bash
   # For local backend (default)
   REACT_APP_API_URL=http://127.0.0.1:8000
   
   # For deployed backend (e.g., Render.com)
   REACT_APP_API_URL=https://your-backend-url.onrender.com
   ```

2. **Install dependencies** (if not already done):
   ```bash
   npm install
   ```

3. **Start the development server**:
   ```bash
   npm start
   ```

4. **Open your browser**:
   - The app will automatically open at `http://localhost:3000`
   - Make sure your backend is running at the URL specified in `.env.local`

## Environment Variables

React apps use environment variables prefixed with `REACT_APP_`. The app will read:
- `.env.local` (highest priority, not committed to git)
- `.env.development.local` (for development)
- `.env.production.local` (for production builds)

## Changing Backend URL

To switch between local and deployed backend:

1. **Edit `.env.local`**:
   ```bash
   # For local backend
   REACT_APP_API_URL=http://127.0.0.1:8000
   
   # For deployed backend
   REACT_APP_API_URL=https://paxos-backend.onrender.com
   ```

2. **Restart the dev server** (stop with Ctrl+C and run `npm start` again)

## Debugging Tips

- Check the browser console (F12) for API errors
- Verify the backend URL in Network tab
- Check that CORS is properly configured on the backend
- Use the React DevTools extension for component debugging

## Troubleshooting

**Backend not connecting?**
- Verify the backend URL in `.env.local` is correct
- Check that the backend server is running
- Verify CORS settings on the backend allow `http://localhost:3000`

**Environment variable not working?**
- Make sure the variable name starts with `REACT_APP_`
- Restart the dev server after changing `.env.local`
- Check that `.env.local` is in the `paxos-visualizer-ui` directory

