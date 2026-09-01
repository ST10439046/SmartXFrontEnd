# Smart-X Frontend - Quick Start Guide

## 🚀 Getting Started

### Prerequisites
- Node.js 16+ with npm
- .NET 10 backend running on `http://localhost:5231`
- Visual Studio Community 2026 (optional, for backend development)

---

## ⚡ Quick Start (60 seconds)

### 1. Install Dependencies
```bash
npm install --legacy-peer-deps
```

### 2. Start Development Server
```bash
npm run dev
```
The app will be available at **`http://localhost:59760`**

### 3. Build for Production
```bash
npm run build
```
Output: `dist/` folder with optimized build

### 4. Preview Production Build
```bash
npm run preview
```

---

## 📊 App Features

### ✅ Active: Sensor Data & Telemetry
- **View live sensors** from database
- **Monitor real-time readings** (auto-refresh every 10 seconds)
- **Register new sensors** with the form
- **Upload attachments** (photos, configs, logs, documents)
- **Track active alerts** with severity indicators
- **Online/offline status** for each sensor

### ⏳ Coming Soon: Command Stream
- Execute commands on sensors
- Track command history
- View command responses

### ⏳ Coming Soon: Network Topology
- Visualize sensor network
- Manage mesh routing
- Monitor link quality

---

## 🔌 API Integration

### Backend Connection
- **Backend URL**: `http://localhost:5231`
- **Frontend URL**: `http://localhost:59760`
- **Auto-refresh**: Every 10 seconds
- **Logging**: All API calls logged in browser console

### Available Endpoints
| Endpoint | Status | Usage |
|----------|--------|-------|
| `/api/sensor` | ✅ Active | Display sensors, register new |
| `/api/telemetrydata` | ✅ Active | Real-time readings |
| `/api/activealert` | ✅ Active | Alert monitoring |
| `/api/attachment` | ✅ Active | File upload/management |
| `/api/commandhistory` | 🔧 Ready | Ready for Commands tab |
| `/api/networktopology` | 🔧 Ready | Ready for Topology tab |

---

## 📝 Registering a New Sensor

### Steps:
1. Go to **Dashboard** → **Register New Sensor** tab
2. Fill in required fields:
   - **Sensor Name**: e.g., "Temperature Sensor 1"
   - **MAC Address**: Format `XX:XX:XX:XX:XX:XX` (e.g., `00:1A:2B:3C:4D:5E`)
   - **Zone**: e.g., "Office", "Front Door"
   - **Node ID**: e.g., "1", "2"
   - **Category**: Select from dropdown (temperature, humidity, power, motion, light, pressure)
   - **Location** (optional): e.g., "Wall Mount", "Desk"
   - **Room** (optional): e.g., "Room 101"

3. **Upload files** (optional):
   - Drag & drop or click to select files
   - Supported: Photos (.jpg, .png), Configs (.json, .xml, .csv), Logs (.log, .txt), Documents (.pdf)

4. Click **Register Sensor**

5. **Success!** Check dashboard - new sensor appears within 10 seconds

---

## 🔍 Testing the Integration

### View Console Logs
1. Open app at `http://localhost:59760`
2. Press **F12** to open DevTools
3. Click **Console** tab
4. Watch for API logs:
   ```
   API Request: GET /api/sensor
   API Response: 200 [{...}]
   ```

### Test API Directly
```bash
# Get all sensors
curl http://localhost:5231/api/sensor

# Get all telemetry
curl http://localhost:5231/api/telemetrydata

# Get all alerts
curl http://localhost:5231/api/activealert
```

### Expected Sensor Response
```json
[
  {
	"macaddress": "00:1A:2B:3C:4D:5E",
	"sensorName": "Door Sensor 1",
	"zone": "Front Door",
	"category": "power_consumption",
	"location": "Main Door",
	"isActive": true,
	"registeredAt": "2026-08-16T15:36:20.650Z",
	"telemetryData": []
  }
]
```

---

## 🎨 UI Components

### Dashboard Components
- **Sensor Cards**: Display sensor info + latest telemetry
- **Alert Feed**: Show active alerts with color severity
- **Loading Spinner**: Appears while fetching data
- **Toast Notifications**: Success/error feedback

### Form Components
- **SensorRegistrationForm**: Create new sensors
- **FileUpload**: Drag-drop file selector
- **Tabs**: Switch between Overview and Register tabs

### Navigation
- **Sidebar**: Three-pillar architecture
  - ✅ Sensor Data (Active)
  - ⏳ Commands (Coming Soon)
  - ⏳ Topology (Coming Soon)

---

## 🛠️ Development

### Project Structure
```
src/
├── components/          # React components
│   ├── Sidebar.jsx
│   ├── TelemetryDashboard.jsx
│   ├── SensorRegistrationForm.jsx
│   ├── FileUpload.jsx
│   ├── Toast.jsx
│   ├── Tabs.jsx
│   └── SensorCard.jsx
├── pages/              # Page components
│   ├── DashboardPage.jsx
│   └── ComingSoonPage.jsx
├── services/           # API layer
│   └── apiClient.js
├── hooks/              # Custom React hooks
│   └── index.js
├── App.jsx             # Main app shell
├── main.jsx            # React entry point
└── index.css           # Global styles
```

### Modify API Base URL
Edit `src/services/apiClient.js`:
```javascript
const API_BASE_URL = 'http://localhost:5231'; // Change this
```

### Add New API Endpoint
```javascript
// In src/services/apiClient.js
export const myApi = {
  getAll: async () => apiClient.get('/api/myendpoint'),
  create: async (data) => apiClient.post('/api/myendpoint', data),
};
```

### Update Component to Use New API
```javascript
// In your component
import { myApi } from '../services/apiClient';

const MyComponent = () => {
  useEffect(() => {
	myApi.getAll().then(res => {
	  // Handle response
	});
  }, []);
};
```

---

## 🐛 Troubleshooting

### Issue: App won't start
```bash
# Clear node_modules and reinstall
rm -r node_modules package-lock.json
npm install --legacy-peer-deps
npm run dev
```

### Issue: Port 59760 is in use
- Vite automatically tries the next available port
- Check the terminal output for the actual port
- Or kill the process using the port

### Issue: Backend connection error
```
Error: API Error: 500 Cannot connect to http://localhost:5231
```
**Solution**: Ensure .NET backend is running on port 5231

### Issue: CORS errors
**Solution**: The backend needs CORS configured for `http://localhost:59760`

### Issue: 404 Not Found errors
**Solution**: Verify API endpoint exists on backend and parameters are correct

### Issue: Sensor doesn't appear after registration
**Solution**: 
- Check browser console for error messages
- Verify MAC address is correctly formatted
- Wait a few seconds - dashboard refreshes every 10 seconds

---

## 📚 Documentation Files

| File | Purpose |
|------|---------|
| `INTEGRATION_SUMMARY.md` | Complete integration overview |
| `API_INTEGRATION_REFERENCE.md` | Detailed API endpoint reference |
| `COMPONENT_DOCUMENTATION.md` | Component API documentation |
| `SETUP_GUIDE.md` | Detailed setup instructions |
| `README.md` | Project overview |
| `QUICK_START.md` | This file |

---

## 🔐 Environment Variables (Optional)

Create `.env.local` to override defaults:
```
VITE_API_BASE_URL=http://localhost:5231
VITE_APP_NAME=Smart-X
```

Use in code:
```javascript
const apiUrl = import.meta.env.VITE_API_BASE_URL || 'http://localhost:5231';
```

---

## 📦 Dependencies

| Package | Version | Purpose |
|---------|---------|---------|
| react | 19.2.8 | UI library |
| vite | 8.2.1 | Build tool |
| axios | 1.7.2 | HTTP client |
| react-router-dom | 6.25.0 | Routing |
| tailwindcss | 4.0.0 | CSS framework |
| lucide-react | 0.395.0 | Icons |

Install specific version:
```bash
npm install package-name@version --legacy-peer-deps
```

---

## 🚀 Deployment

### Build for Production
```bash
npm run build
```

### Deploy to Production
1. Copy `dist/` folder to web server
2. Configure backend CORS for production domain
3. Update `API_BASE_URL` in apiClient.js to production backend
4. Test all API endpoints work with production backend

### Using Docker (Optional)
```dockerfile
FROM node:18-alpine
WORKDIR /app
COPY package*.json ./
RUN npm install --legacy-peer-deps
COPY . .
RUN npm run build
EXPOSE 5173
CMD ["npm", "run", "preview"]
```

---

## 💡 Tips & Tricks

### Hot Reload Development
- Save any file and it auto-reloads in the browser
- Vite is incredibly fast - changes appear instantly

### Debug API Requests
1. Open DevTools (F12)
2. Go to **Network** tab
3. Filter by XHR/Fetch
4. Click any request to see details

### Enable Dark Mode
- Dashboard respects system preference
- Dark mode classes: `dark:bg-gray-800`, `dark:text-white`

### Add Console Logging
```javascript
// In any component
console.log('API Response:', response.data);
console.error('Error:', error);
```

### Performance Tips
- Dashboard auto-refreshes every 10 seconds
- Reduce this interval if needed: Change `10000` to `5000` in TelemetryDashboard.jsx
- Increase for slower connections: Change to `30000`

---

## ❓ FAQ

**Q: How often does data refresh?**
A: Every 10 seconds automatically. The dashboard fetches sensors, telemetry, and alerts.

**Q: Can I edit/delete sensors?**
A: Delete is in the API layer. UI components for edit/delete can be added to SensorCard.

**Q: Where are files uploaded stored?**
A: Files are uploaded via `/api/attachment` endpoint and stored on the backend.

**Q: Can I add more pillars?**
A: Yes! Add routes in `App.jsx` and create new page components in `src/pages/`.

**Q: How do I add dark mode toggle?**
A: Tailwind dark mode is already available. Add a button that toggles `dark` class on document.

**Q: What's the auth/login?**
A: Currently no authentication. Add it by:
1. Creating a login component
2. Storing token in localStorage
3. Adding token to API interceptor headers
4. Protecting routes with PrivateRoute component

---

## 📞 Support

**For Issues**:
1. Check browser console (F12 → Console)
2. Check Network tab for API errors
3. Review error logs in `INTEGRATION_SUMMARY.md`
4. Verify backend is running: `http://localhost:5231`

**For Questions**:
- Review `API_INTEGRATION_REFERENCE.md` for API details
- Check `COMPONENT_DOCUMENTATION.md` for component usage
- Inspect source code - well-commented

---

## ✅ Next Steps

1. **Test it out** - Start the app and register a sensor
2. **Monitor telemetry** - Watch live data updates
3. **Explore APIs** - Call endpoints directly with curl
4. **Customize UI** - Modify colors, layouts, components
5. **Add features** - Implement command history or topology tabs
6. **Deploy** - Move to production when ready

---

**Happy coding! 🎉**

Last Updated: Integration Complete  
Status: Production Ready ✅  
Version: 1.0
