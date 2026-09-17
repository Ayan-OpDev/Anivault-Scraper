# Original Railway Deployment Details

The application was originally configured for deployment on Railway with the following settings (extracted from `railway.toml`):

- **Service Name:** `anivault-api`
- **Builder Environment:** Nixpacks
- **Node.js Version:** 20 (`nodejs_20`)
- **Build Command:** `npm install && npm run build`
- **Start Command:** `npm start`
- **Restart Policy:** Automatically restarts on failure (up to 3 retries)

*Note: The original `railway.toml` file was removed as the app has been migrated to be Vercel-ready / AI Studio compliant.*
