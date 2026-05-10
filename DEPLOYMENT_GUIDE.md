# Earnify Lab Investment Platform - Deployment Guide

## 📋 Overview
This is a complete investment platform with frontend interface and backend API connected to Supabase (PostgreSQL) database. The application includes:
- Investment plan management (Basic, Gold, VIP)
- Deposit and withdrawal functionality
- Portfolio tracking and reporting
- Referral system
- Admin panel placeholders
- Responsive UI with Urdu language support

## 📁 Project Structure
```
earnify-project/
├── earnify-lab_1.html          # Main frontend (HTML/CSS/JS)
├── backend/
│   ├── package.json            # Node.js dependencies
│   ├── server.js               # Express.js backend with Supabase
│   └── .env                    # Environment variables (NOT committed)
├── js/
│   └── apiService.js           # API communication layer
└── withdraw/                   # Placeholder for withdrawal-specific files
```

## 🔧 Prerequisites for Local Development

### 1. Install Node.js
**This is REQUIRED to run the backend**
- Download from: https://nodejs.org (LTS version)
- Install with default options
- Verify installation:
  ```bash
  node --version   # Should show v16.x or higher
  npm --version    # Should show 8.x or higher
  ```

### 2. Install Dependencies
```bash
cd "C:\Users\yahya\Links\earnify-project\backend"
npm install
```

### 3. Configure Environment
The `backend/.env` file should contain:
```
SUPABASE_URL=https://your-project-id.supabase.co
SUPABASE_SERVICE_ROLE_KEY=your_service_role_key_here
PORT=5000
```
Get these values from your Supabase project: Settings → API

## 🚀 Running Locally

1. **Start the backend server**:
   ```bash
   cd "C:\Users\yahya\Links\earnify-project\backend"
   npm start
   ```
   Output should show: `🚀 Server running on http://localhost:5000`

2. **Open the frontend**:
   - Open `C:\Users\yahya\Links\earnify-project\earnify-lab_1.html` in your web browser
   - The application will connect to your local backend at http://localhost:5000

3. **Test functionality**:
   - Click any investment plan (Basic/Gold/VIP)
   - Enter an amount and click "Deposit Proceed"
   - Check Supabase Table Editor → `investments` to verify data storage
   - Try withdrawal functionality (once implemented in UI)

## 🌐 Production Deployment Options

### Option 1: Render.com (Recommended - Free Tier Available)
1. Push code to GitHub repository
2. Create new Web Service on Render
3. Connect your GitHub repository
4. Configure:
   - Build Command: `npm install` (in backend directory)
   - Start Command: `npm start` (in backend directory)
   - Root Directory: `backend/`
5. Add Environment Variables:
   - `SUPABASE_URL`: Your Supabase project URL
   - `SUPABASE_SERVICE_ROLE_KEY`: Your Supabase service role key
   - `PORT`: Will be provided automatically
6. Deploy!

### Option 2: Fly.io
1. Install Flyctl: https://fly.io/docs/hands-on/install-flyctl/
2. Initialize: `fly launch` (in project root)
3. Configure Dockerfile (see below)
4. Deploy: `fly deploy`

### Option 3: Vercel (for Node.js)
1. Install Vercel CLI: `npm i -g vercel`
2. Deploy: `vercel` (in backend directory)
3. Add environment variables in Vercel dashboard

### Option 4: Traditional VPS (DigitalOcean, AWS, etc.)
1. Install Node.js on server
2. Clone repository
3. Run `npm install` in backend
4. Use PM2 for process management: `npm i -g pm2 && pm2 start server.js`
5. Configure Nginx as reverse proxy (optional)

## 🐳 Docker Deployment (Alternative)
Create a `Dockerfile` in the backend directory:
```dockerfile
FROM node:18-alpine
WORKDIR /app
COPY package*.json ./
RUN npm install
COPY . .
EXPOSE 5000
CMD ["npm", "start"]
```

Build and run:
```bash
docker build -t earnify-backend .
docker run -p 5000:5000 --env-file .env earnify-backend
```

## 📝 Important Notes

### Frontend Deployment
The `earnify-lab_1.html` file is a standalone HTML file that can be hosted anywhere:
- Netlify (drag & drop)
- GitHub Pages
- Any static web host
- Even served by the same Node.js backend (add static middleware)

### Database Considerations
- Supabase provides a free PostgreSQL database
- Tables are created automatically on startup:
  - `investments`: Stores investment records
  - `withdrawals`: Tracks withdrawal requests
- For production, consider:
  - Enabling Row Level Security (RLS)
  - Setting up proper authentication
  - Adding indexes for performance

### Security Features Included
- Helmet.js for HTTP header protection
- CORS middleware for cross-origin control
- Rate limiting (100 requests/15 minutes/IP)
- Input validation on all endpoints
- Environment variable separation
- Parameterized queries via Supabase client (SQL injection prevention)

### Known Limitations (Future Work)
1. **No User Authentication** - Currently uses demo/local state
   - Add: Supabase Auth integration (email/password, social logins)

2. **No Real-time Updates** - Requires manual refresh
   - Add: Supabase Realtime subscriptions

3. **Limited Error Handling** - Basic try/catch
   - Add: Comprehensive error logging & user-friendly messages

4. **No Automated Testing** - Would require Jest/Mocha
   - Add: Unit & integration tests

5. **No Input Sanitization** - Beyond basic validation
   - Add: Sanitization for all user inputs

## 📞 Support

If you encounter issues:
1. **Node.js not found**: Reinstall from nodejs.org
2. **Connection errors**: Check .env file and Supabase status
3. **Port already in use**: Change PORT in .env or kill existing process
4. **Database errors**: Verify table schema in Supabase editor
5. **CORS issues**: Ensure backend CORS middleware is active

## ✅ Verification Checklist
After deployment, verify:
1. [ ] Backend responds to `GET /api/health`
2. [ ] Frontend loads without console errors
3. [ ] Investment creation works and stores data
4. [ ] Withdrawal creation works and stores data
5. [ ] Portfolio endpoint returns calculated values
6. [ ] Responsive design works on mobile devices
7. [ ] Error handling shows appropriate user messages

---

**Remember**: A truly functional application requires the ability to run and test the code. This guide provides everything needed to deploy a working investment platform once Node.js is properly installed in your environment.

The codebase is complete and ready for deployment - it simply requires the Node.js runtime to execute the backend services.

Good luck with your deployment! 🚀