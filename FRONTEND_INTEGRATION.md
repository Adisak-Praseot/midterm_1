# Frontend Integration Guide

## Setup Complete ✅

The Quasar frontend has been successfully updated to connect with the Express backend API.

### What's Been Updated

#### 1. **IndexPage.vue** - Enhanced UI Components
   - **Git Workflow Section**: Displays 5 steps of the Git workflow
   - **Docker Concepts Section**: Shows key Docker concepts
   - **API Data Section**: Fetches and displays data from the backend `/api/demo` endpoint

#### 2. **Environment Variables** (.env)
```
API_URL=http://localhost:3000
```

The frontend will use this URL to make API calls to the backend.

#### 3. **Axios Configuration**
- Axios is already installed and configured as a boot file
- The IndexPage component imports axios directly for API calls
- API_URL is read from environment variables using `process.env.API_URL`

### Feature Implementation

#### **API Data Fetching**
The `fetchData()` function:
```javascript
const fetchData = async () => {
  apiLoading.value = true;
  apiError.value = '';

  try {
    const response = await axios.get(API_URL + '/api/demo');
    apiData.value = response.data;
  } catch (error) {
    console.error('API Error:', error);
    apiError.value = 'ไม่สามารถโหลดข้อมูลจาก API ได้';
  } finally {
    apiLoading.value = false;
  }
};
```

- Fetches data from `http://localhost:3000/api/demo`
- Shows a loading spinner while fetching
- Displays error message if API call fails
- Has a "Refresh Data" button to manually trigger API calls

#### **Data Display**
The fetched data is displayed in the "Data from Backend API" card:
- **Advanced Git**: Shows Git workflow details from the API
- **Advanced Docker**: Shows Docker concepts from the API

### Backend API Response Format
The backend at `/api/demo` returns:
```json
{
  "git": {
    "title": "Advanced Git Workflow",
    "detail": "ใช้ branch protection บน GitHub, code review ใน PR, และ squash merge เพื่อ history สะอาด"
  },
  "docker": {
    "title": "Advanced Docker",
    "detail": "ใช้ multi-stage build, healthcheck ใน Dockerfile, และ orchestration ด้วย Compose/Swarm"
  }
}
```

### How to Run

#### 1. **Start Backend** (if not running)
```bash
cd backend
node server.js
```
Backend will run on http://localhost:3000

#### 2. **Start Frontend**
```bash
cd frontend
npm run dev
```
Frontend will run on http://localhost:9000 (default Quasar dev port)

#### 3. **Access the Application**
Open browser and navigate to `http://localhost:9000`

The page should load and automatically fetch data from the backend API. You should see:
- Git Workflow steps displayed
- Docker Concepts listed
- Data from Backend API section with the API response

### Troubleshooting

**Issue**: API data not loading
- **Check**: Backend is running on port 3000
- **Check**: CORS is enabled in backend (already configured)
- **Check**: Browser console for error messages

**Issue**: 404 error on API endpoint
- **Check**: Backend `/api/demo` endpoint exists
- **Check**: API_URL in .env matches backend URL

**Issue**: Loading spinner stays stuck
- **Check**: Network tab in browser DevTools for failed requests
- **Check**: Backend logs for errors

### File Structure
```
frontend/
├── .env                           # Environment variables
├── .env.example                   # Example env template
├── src/
│   ├── boot/
│   │   └── axios.js              # Axios configuration
│   ├── pages/
│   │   └── IndexPage.vue          # UPDATED - Main page with API integration
│   └── ...
└── ...
```

### CORS Configuration
The backend has CORS enabled, which allows the frontend (running on a different port) to make requests to the backend API. This is already configured in `backend/server.js`:

```javascript
app.use(cors());  // อนุญาต cross-origin จาก frontend
```

### Production Considerations

For production deployment:
1. Update `API_URL` to point to the production backend
2. Consider using environment-specific configuration
3. Implement proper error handling and user feedback
4. Add request/response interceptors for authentication if needed
5. Implement caching to reduce API calls
