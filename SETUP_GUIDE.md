# Smart-X IoT Frontend - Setup & Quick Start Guide

## ✅ Build Status
**Build: SUCCESSFUL** ✓

## 🎉 Project Initialization Complete

Your Smart-X IoT React frontend has been successfully generated with all required features and components.

---

## 📋 What's Included

### ✨ Features Implemented

#### 1. **Navigation & Layout** ✓
- Sidebar navigation with three architectural pillars
- "Sensor Data Ingestion and Telemetry" - **ACTIVE**
- "Real-Time Command Stream and History" - Coming Soon
- "Network Topology and Mesh Routing" - Coming Soon
- Responsive mobile-friendly design
- Dark mode support

#### 2. **Sensor Registration Form** ✓
- MAC Address input with format validation (XX:XX:XX:XX:XX:XX)
- Deployment location fields (Room, Zone, Node ID)
- Category dropdown (Environmental, Power Consumption, Actuator)
- Optional description field
- Form validation and error feedback
- Success/error toast notifications

#### 3. **File Upload Mechanism** ✓
- Drag & drop file upload interface
- Multiple file selection support
- File type and size validation (50MB max)
- Progress tracking for uploads
- Visual status indicators
- Optimized for configuration files, photos, and logs

#### 4. **API Integration** ✓
- Axios configuration with interceptors
- TelemetryPacket<T> structure for C# backend compatibility
- RESTful API methods for sensors and telemetry
- Error handling and response parsing
- Automatic timestamp and metadata injection

#### 5. **Real-time Visual Telemetry Dashboard** ✓
- Live sensor data cards with current, min, max, average values
- Real-time data updates (5-second refresh)
- Sensor status indicators (Online/Offline)
- Network status panel
- Active alerts and notifications
- Trend indicators for each sensor

#### 6. **UI/UX with Tailwind CSS** ✓
- Professional, responsive design
- Crisp feedback loops with toast notifications
- Smooth animations and transitions
- Consistent color scheme
- Accessibility features (ARIA labels, semantic HTML)
- Mobile, tablet, and desktop optimization

---

## 🚀 Getting Started

### Prerequisites
- Node.js 16+ installed
- .NET 10 Web API running on `http://localhost:5231`

### Step 1: Install Dependencies
```bash
npm install --legacy-peer-deps
```

### Step 2: Start Development Server
```bash
npm run dev
```

The application will be available at `http://localhost:59759` (or your configured Vite port).

### Step 3: Test Backend Connection
1. Ensure your .NET 10 API is running
2. Navigate to the Dashboard
3. Check the sidebar footer for "API Connected" status
4. Try registering a sensor to verify API communication

---

## 📁 Project Structure

```
src/
├── components/                      # Reusable UI components
│   ├── FileUpload.jsx              # Drag & drop file uploader
│   ├── SensorRegistrationForm.jsx   # Sensor registration form
│   ├── Sidebar.jsx                 # Navigation sidebar
│   ├── Tabs.jsx                    # Tab component system
│   ├── TelemetryDashboard.jsx       # Real-time dashboard
│   ├── Toast.jsx                   # Notification system
│   └── index.js                    # Component exports
│
├── hooks/                           # Custom React hooks
│   └── index.js                    # useNotification, useForm, useAsync, useFileUpload
│
├── pages/                           # Page components
│   ├── DashboardPage.jsx           # Main dashboard with tabs
│   └── ComingSoonPage.jsx          # Placeholder for future features
│
├── services/                        # API and external services
│   └── apiClient.js                # Axios API client
│
├── App.jsx                          # Main app component with routing
├── main.jsx                         # React entry point
└── index.css                        # Tailwind CSS configuration

Configuration Files:
├── tailwind.config.js               # Tailwind CSS theme config
├── postcss.config.js                # PostCSS configuration
├── vite.config.js                   # Vite build configuration
└── package.json                     # Dependencies and scripts
```

---

## 🎯 Key Components

### Sidebar Navigation
- Displays three architectural pillars
- Active/disabled states
- Mobile responsive with hamburger menu
- Shows API connection status

### Sensor Registration Form
- Complete form with validation
- Real-time feedback
- File attachment support
- Auto-submit handling with loading states

### File Upload Component
- Drag & drop interface
- File validation
- Progress tracking
- Error messages

### Telemetry Dashboard
- Real-time sensor cards
- Live data updates
- Alert panel
- Network status indicators

### Toast Notification System
- Success, error, warning, info types
- Auto-dismiss or manual control
- Stacked notifications

---

## 🔌 API Integration

### Base URL
```
http://localhost:5231
```

### Example TelemetryPacket Structure
```javascript
{
  timestamp: "2024-08-10T13:06:00Z",
  data: {
	// Your sensor or telemetry data
  },
  metadata: {
	version: "1.0",
	source: "web-frontend",
	action: "sensor_registration",
	sensorId: "optional-id"
  }
}
```

### Available Endpoints
- `POST /api/sensors/register` - Register new sensor
- `GET /api/sensors` - List all sensors
- `POST /api/sensors/upload` - Upload sensor files
- `GET /api/telemetry/{sensorId}` - Get telemetry
- `POST /api/telemetry/submit` - Submit telemetry
- `GET /health` - Health check

---

## 🎨 Customization

### Colors & Theme
Edit `tailwind.config.js` to customize:
- Primary color scheme
- Custom animations
- Typography scale
- Spacing values

### API Endpoints
Modify `src/services/apiClient.js` to:
- Change API base URL
- Add authentication headers
- Update endpoint paths
- Modify request/response handling

### Form Validation
Update `src/components/SensorRegistrationForm.jsx` to:
- Add/remove form fields
- Customize validation rules
- Change error messages

---

## 📊 Usage Workflow

### Registering a Sensor

1. **Navigate to Dashboard**
   - Go to "Sensor Data Ingestion and Telemetry"

2. **Click "Register Sensor" Tab**
   - Fill in sensor details:
	 - Sensor Name: "Kitchen Temperature Sensor"
	 - MAC Address: "00:1A:2B:3C:4D:5E"
	 - Location: "Kitchen"
	 - Zone: "Zone A"
	 - Node ID: "NODE-001"
	 - Category: "Environmental"

3. **Upload Files (Optional)**
   - Drag configuration files or click to browse
   - Supported: `.json`, `.xml`, `.csv`, `.log`, `.jpg`, `.png`, `.pdf`

4. **Submit**
   - Click "Register Sensor"
   - See success notification
   - View sensor in telemetry overview

### Monitoring Telemetry

1. **View Dashboard Overview**
   - See real-time sensor cards
   - Monitor live values and trends
   - Check alert status

2. **Monitor Alerts**
   - Critical alerts appear in red
   - Warnings in yellow
   - Info messages in blue

3. **Network Status**
   - Bottom right: API connection status
   - Data throughput and uptime metrics

---

## 🛠️ Development Commands

```bash
# Start development server with hot reload
npm run dev

# Build for production
npm run build

# Preview production build
npm run preview

# Run ESLint
npm run lint

# Install dependencies (if not already done)
npm install --legacy-peer-deps
```

---

## 🔐 Security Notes

✓ MAC address validation prevents malformed entries
✓ File type and size validation on upload
✓ Input sanitization for form fields
✓ No sensitive data in localStorage
✓ CORS configured for backend API

---

## 🌐 Browser Support

- Chrome 90+
- Firefox 88+
- Safari 14+
- Edge 90+

---

## 📱 Responsive Breakpoints

- **Mobile**: < 640px
- **Tablet**: 640px - 1024px
- **Desktop**: > 1024px

---

## 🧪 Testing the Application

### Manual Testing Checklist

- [ ] Sidebar navigation works
- [ ] Active pillar loads dashboard
- [ ] Disabled pillars show "Coming Soon"
- [ ] Sensor registration form validates
- [ ] MAC address validation works
- [ ] File upload accepts drag & drop
- [ ] Form submission shows loading state
- [ ] Success/error notifications appear
- [ ] Dark mode toggle works
- [ ] Mobile layout responsive
- [ ] Telemetry dashboard updates live
- [ ] Alerts display correctly
- [ ] API connection status shows

---

## 🐛 Troubleshooting

### Issue: Backend Connection Failed
**Solution:**
- Verify .NET API is running on `http://localhost:5231`
- Check browser console for network errors
- Ensure CORS is enabled on backend

### Issue: Dependencies Installation Failed
**Solution:**
```bash
npm install --legacy-peer-deps
npm audit fix
```

### Issue: Form Validation Errors
**Solution:**
- MAC address must be: `XX:XX:XX:XX:XX:XX`
- All required fields must be filled
- Check validation messages in form

### Issue: File Upload Not Working
**Solution:**
- Files must be under 50MB
- Check supported file extensions
- Verify backend upload endpoint

---

## 📚 Next Steps

1. **Test with Backend API**
   - Register test sensors
   - Submit telemetry data
   - Verify database integration

2. **Customize for Your Needs**
   - Update color scheme
   - Add custom validation rules
   - Extend form fields

3. **Deploy to Production**
   ```bash
   npm run build
   # Deploy dist/ folder to your hosting
   ```

4. **Implement Future Pillars**
   - Command Stream & History
   - Network Topology & Mesh Routing

---

## 📞 Support

For issues or questions:
1. Check the console for error messages
2. Review `SMART_X_README.md` for detailed documentation
3. Verify backend API connectivity
4. Check browser DevTools Network tab

---

## 🎓 Learn More

- [React Documentation](https://react.dev)
- [Tailwind CSS](https://tailwindcss.com)
- [React Router](https://reactrouter.com)
- [Axios](https://axios-http.com)
- [Lucide Icons](https://lucide.dev)

---

## ✨ Happy Building!

Your Smart-X IoT Frontend is ready for development. Start the dev server and begin creating amazing IoT experiences!

```bash
npm run dev
```

🚀 **Build Status**: ✅ SUCCESSFUL
📦 **Dependencies**: ✅ INSTALLED
🎨 **Styling**: ✅ CONFIGURED (Tailwind CSS)
🔌 **API**: ✅ READY (Axios configured)
🎯 **Features**: ✅ COMPLETE

---

*Generated: 2024-08-10*
*Framework: React 19.2.8 + Vite + Tailwind CSS*
*Backend: .NET 10 Web API*
