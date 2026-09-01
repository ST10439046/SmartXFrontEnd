# 🚀 Smart-X IoT Frontend - Quick Reference Guide

## ⚡ Quick Start (2 Minutes)

```bash
# 1. Install dependencies (first time only)
npm install --legacy-peer-deps

# 2. Start development server
npm run dev

# 3. Open in browser
# http://localhost:59759
```

✅ That's it! Your app is running.

---

## 📍 What You'll See

### Dashboard (Default)
- **Sidebar**: Three architectural pillars (active telemetry, locked features)
- **Main Area**: Two tabs
  - "Telemetry Overview" - Live sensor data
  - "Register Sensor" - New sensor form

### Telemetry Overview
- Real-time sensor cards
- Alert notifications
- Network status

### Register Sensor Tab
- Form with validation
- File upload area
- Submit & Clear buttons

---

## 🎯 Quick Navigation

| Action | Path | How |
|--------|------|-----|
| View Dashboard | `/dashboard` | Click "Sensor Data..." in sidebar |
| Register Sensor | `/dashboard` | Dashboard → "Register Sensor" tab |
| Command Stream | `/commands` | Click (locked) in sidebar |
| Topology | `/topology` | Click (locked) in sidebar |

---

## 📝 Form Fields Reference

### Sensor Registration Form

**Required Fields:**
```
Sensor Name:    "Kitchen Temperature Sensor"
MAC Address:    "00:1A:2B:3C:4D:5E"  (format required)
Location:       "Kitchen"
Zone:           "Zone A"
Node ID:        "NODE-001"
Category:       "Environmental" | "Power Consumption" | "Actuator"
```

**Optional Fields:**
```
Description:    "Any additional notes"
Files:          Configuration, photos, or logs
```

---

## 🔌 API Configuration

**Base URL (in `src/services/apiClient.js`):**
```javascript
const API_BASE_URL = 'http://localhost:5231';
```

**To change endpoint:**
1. Edit `src/services/apiClient.js` line 3
2. Update `API_BASE_URL`
3. Restart dev server

---

## 🎨 Customization Quick Links

### Change Colors
**File**: `tailwind.config.js`
```javascript
colors: {
  primary: { 500: '#0ea5e9' },      // Change this
  success: { 500: '#10b981' },      // Or this
  warning: { 500: '#f59e0b' },      // Or this
  danger: { 500: '#ef4444' }        // Or this
}
```

### Change Form Fields
**File**: `src/components/SensorRegistrationForm.jsx`
- Add input after line 70
- Update validation in `validate` function
- Add API field in form data object

### Change Dashboard Data
**File**: `src/components/TelemetryDashboard.jsx`
- Modify `sensors` state (line 104+)
- Update alert types (line 155+)

---

## 🧪 Common Testing Scenarios

### Test 1: Register a Sensor
1. Go to "Register Sensor" tab
2. Fill: Name, MAC (00:1A:2B:3C:4D:5E), Location, Zone, Node, Category
3. Click "Register Sensor"
4. ✅ Should see "Sensor registered successfully!"

### Test 2: Invalid MAC Address
1. Enter invalid MAC: "12:34:56"
2. ✅ Should show error: "Invalid MAC Address format"

### Test 3: File Upload
1. Drag a `.json` file to upload area
2. ✅ File appears in list
3. Click "Register Sensor"
4. ✅ File uploads with sensor

### Test 4: API Connection
1. Stop backend API
2. Try to register sensor
3. ✅ Should show error notification
4. Start backend API
5. ✅ Should work again

---

## 🔧 Development Tips

### View React Components
- Install: React DevTools browser extension
- Inspect components, props, state in DevTools

### View API Calls
1. Open browser DevTools (F12)
2. Go to "Network" tab
3. Register sensor or upload file
4. See requests to API

### Enable Debug Logging
**In `src/services/apiClient.js`**, uncomment line for logging:
```javascript
console.log('API Request:', config);
```

### Auto-reload on Save
- Vite has Hot Module Replacement
- Just save file, browser updates automatically

---

## 📱 Mobile Testing

### Desktop Browser (Recommended)
- DevTools → Device Toolbar (Ctrl+Shift+M)
- Select iPhone or other device
- Test responsive design

### Real Phone
1. Get your machine IP: `ipconfig` (Windows) or `ifconfig` (Mac/Linux)
2. Start dev server
3. Open `http://YOUR_IP:59759` on phone
4. Test on real device

---

## 🐛 Quick Debug Checklist

**App not loading?**
- [ ] Is npm dev server running?
- [ ] Check console for errors (F12)
- [ ] Try `npm install --legacy-peer-deps`

**Backend not connecting?**
- [ ] Is .NET API running on port 5231?
- [ ] Check Network tab in DevTools
- [ ] Is CORS enabled on backend?

**Form not validating?**
- [ ] Check validation function in component
- [ ] Look for error messages in console
- [ ] Verify field names match

**Styles not applying?**
- [ ] Check Tailwind classes are spelled right
- [ ] Restart dev server
- [ ] Clear browser cache (Ctrl+Shift+Delete)

---

## 📦 Build & Deploy

### Build for Production
```bash
npm run build
```
Creates `dist/` folder with optimized files.

### Test Production Build Locally
```bash
npm run preview
```
Serves production build at `http://localhost:4173`

### Deploy to Vercel (Free)
1. Push code to GitHub
2. Go to vercel.com
3. Import your repo
4. Set `API_BASE_URL` environment variable
5. Deploy! 🚀

---

## 🎯 Component Quick Reference

### Using Toast Notifications
```javascript
import { useNotification } from '../hooks';

const { success, error, warning, info } = useNotification();

// In your code:
success('Great news!');
error('Something wrong!');
warning('Be careful!');
info('FYI...');
```

### Using Form Hook
```javascript
import { useForm } from '../hooks';

const { values, errors, handleChange, handleSubmit } = useForm(
  { name: '', email: '' },
  async (values) => {
	// Handle submission
  },
  (name, value) => {
	// Validation logic
  }
);
```

### Making API Calls
```javascript
import { sensorApi } from '../services/apiClient';

// Register sensor
const response = await sensorApi.registerSensor(data);

// Upload file
await sensorApi.uploadSensorFile(sensorId, file);

// Get telemetry
const telemetry = await telemetryApi.getRealtimeTelemetry(sensorId);
```

---

## 🌐 Environment Variables

Create `.env.local` file in project root:
```env
VITE_API_BASE_URL=http://your-api.com
```

Then use in code:
```javascript
const API_URL = import.meta.env.VITE_API_BASE_URL;
```

---

## 📞 Useful Resources

| Need | Location |
|------|----------|
| Full Docs | `SMART_X_README.md` |
| Setup Help | `SETUP_GUIDE.md` |
| Implementation Details | `IMPLEMENTATION_SUMMARY.md` |
| API Endpoints | `src/services/apiClient.js` |
| Components | `src/components/` |
| Custom Hooks | `src/hooks/index.js` |

---

## ✨ Features Status

| Feature | Status | Notes |
|---------|--------|-------|
| Sidebar Navigation | ✅ Active | Works perfectly |
| Sensor Registration | ✅ Active | Full validation |
| File Upload | ✅ Active | Drag & drop support |
| Real-time Dashboard | ✅ Active | 5-sec updates |
| Telemetry API | ✅ Active | Full integration |
| Notifications | ✅ Active | All types |
| Responsive Design | ✅ Active | Mobile optimized |
| Dark Mode | ✅ Active | Auto-detect |
| Command Stream | 🔒 Pending | Architecture ready |
| Network Topology | 🔒 Pending | Architecture ready |

---

## 🚀 Next Feature Checklist

Ready to add something new?

- [ ] Create new component in `src/components/`
- [ ] Create new page in `src/pages/`
- [ ] Add new API method in `src/services/apiClient.js`
- [ ] Add new hook in `src/hooks/index.js`
- [ ] Add route in `src/App.jsx`
- [ ] Test in dev server
- [ ] Build & verify

---

## 💡 Pro Tips

1. **Performance**: Import only what you need
   ```javascript
   import { useNotification } from '../hooks';  // ✅ Good
   import * from '../hooks';                     // ❌ Bad
   ```

2. **Styling**: Use Tailwind utility classes
   ```jsx
   <div className="p-4 bg-white rounded-lg shadow">  // ✅ Good
   <div style={{ padding: '1rem' }}>                 // ❌ Avoid
   ```

3. **State**: Keep state as local as possible
   ```javascript
   const [count, setCount] = useState(0);  // ✅ Local state
   // Context for global state
   ```

4. **Error Handling**: Always catch API errors
   ```javascript
   try {
	 await apiCall();
   } catch (error) {
	 show(error.message, 'error');  // ✅ Good
   }
   ```

---

## 🎓 Learning Path

1. **Read**: `SETUP_GUIDE.md`
2. **Run**: `npm run dev`
3. **Explore**: Open `src/App.jsx`
4. **Test**: Try registering a sensor
5. **Modify**: Change colors in `tailwind.config.js`
6. **Build**: `npm run build`
7. **Deploy**: Follow deployment section

---

## ✅ Launch Checklist

- [ ] Backend API running on 5231?
- [ ] Frontend dev server running? `npm run dev`
- [ ] Can access http://localhost:59759?
- [ ] Sidebar shows three pillars?
- [ ] Can register sensor?
- [ ] Can upload files?
- [ ] Telemetry dashboard works?
- [ ] Notifications appear?
- [ ] Mobile view works?

---

## 🎉 You're All Set!

Your Smart-X IoT Frontend is ready to go. Start developing!

```bash
npm run dev
```

**Happy coding! 🚀**

---

*For detailed documentation, see `SMART_X_README.md`*
*For setup help, see `SETUP_GUIDE.md`*
*For architecture details, see `IMPLEMENTATION_SUMMARY.md`*
