# Sensor CRUD - Quick Reference Card

## 🎯 File Locations

| File | Purpose | Lines |
|------|---------|-------|
| `src/pages/SensorCRUDPage.jsx` | Main CRUD page | 375 |
| `src/components/SensorForm.jsx` | Create/Edit form | 200 |
| `src/components/SensorListTable.jsx` | Sensor table display | 130 |
| `src/components/SensorDeleteDialog.jsx` | Delete confirmation | 95 |
| `src/App.jsx` | Routes (MODIFIED) | Updated |
| `src/components/Sidebar.jsx` | Navigation (MODIFIED) | Updated |

## 🚀 Quick Start

### Step 1: Start the Backend API
```powershell
cd C:\Users\Shuayb\source\repos\SmartIotApi
dotnet run
# Listens on http://localhost:5231
```

### Step 2: Start the Frontend Dev Server
```powershell
cd C:\Users\Shuayb\source\repos\SmartIotFrontEndApp
npm run dev
# Runs on http://localhost:5173
```

### Step 3: Navigate to Sensors
- Click "Sensor Management" in sidebar, OR
- Visit `http://localhost:5173/sensors`

## 📋 Feature Checklist

| Feature | Status | Details |
|---------|--------|---------|
| List Sensors | ✅ Complete | Paginated table with 10 items/page |
| Search | ✅ Complete | Search by MAC, Node ID, Category, Zone, Room |
| Create | ✅ Complete | Form with validation |
| Edit | ✅ Complete | In-line form editing |
| Delete | ✅ Complete | Confirmation dialog |
| Pagination | ✅ Complete | Smart page navigation |
| Dark Mode | ✅ Complete | Auto-detect system theme |
| Mobile | ✅ Complete | Responsive design |
| Validation | ✅ Complete | Real-time error messages |
| Error Handling | ✅ Complete | Toast notifications |

## 🔗 API Endpoints Used

```javascript
// All endpoints at http://localhost:5231/api/sensor

GET     /                          // List all sensors
GET     /{macaddress}              // Get single sensor
POST    /                          // Create sensor
PUT     /{macaddress}              // Update sensor
DELETE  /{macaddress}              // Delete sensor
```

## 📝 Form Fields

| Field | Type | Required | Rules | Example |
|-------|------|----------|-------|---------|
| MAC Address | Text | Yes | Format: XX:XX:XX:XX:XX:XX | 00:1A:2B:3C:4D:5E |
| Node ID | Text | Yes | Alphanumeric | ENV-01 |
| Category | Select | Yes | Dropdown list | Environmental |
| Zone | Text | Yes | Any text | North Plot |
| Room | Text | No | Any text | Greenhouse |
| Active | Checkbox | No | Default: true | true/false |

## 🎨 Category Colors

| Category | Color | Hex |
|----------|-------|-----|
| Environmental | Blue | #3B82F6 |
| Power | Yellow | #EAB308 |
| Actuator | Purple | #A855F7 |
| Temperature | Orange | #F97316 |
| Humidity | Cyan | #06B6D4 |
| Pressure | Gray | #6B7280 |
| Light | Amber | #FBBF24 |
| Motion | Green | #22C55E |
| Other | Slate | #64748B |

## ⌨️ Keyboard Shortcuts

| Key | Action |
|-----|--------|
| `Tab` | Navigate between form fields |
| `Enter` | Submit form or search |
| `Escape` | Close form or dialog |
| `Ctrl+A` | Select all in search field |

## 🔴 Common Issues & Solutions

### Issue: "Failed to fetch sensors"
**Solution:**
1. Check backend is running: `http://localhost:5231/health`
2. Check CORS headers in browser console
3. Verify API URL in `src/services/apiClient.js`

### Issue: Form validation errors
**Solution:**
- MAC Address: Use format `00:1A:2B:3C:4D:5E`
- Fill all required fields (marked with *)
- Node ID must be alphanumeric

### Issue: Edit button doesn't work
**Solution:**
- Ensure sensor MAC address is valid
- Check backend returns sensor data
- Verify Form component imports are correct

### Issue: Delete fails silently
**Solution:**
1. Check browser console for error messages
2. Verify sensor exists on backend
3. Check backend logs for validation errors

### Issue: Search not filtering
**Solution:**
- Ensure sensors are loaded from API
- Try exact field values
- Check search is across all 5 fields

## 📊 Data Flow

```
User Action → React Component → useNotification Hook → 
→ sensorApi (axios) → Backend API → Database → 
→ Response → Component State → UI Update
```

## 🧩 Component Dependencies

```
SensorCRUDPage
├── uses: sensorApi
├── uses: useNotification hook
├── renders: SensorForm (when editing/creating)
├── renders: SensorListTable (when viewing list)
└── renders: SensorDeleteDialog (when deleting)

SensorForm
└── uses: useForm hook
	└── uses: validate function for each field

SensorListTable
├── renders: sensor rows with data
└── provides: onEdit, onDelete callbacks

SensorDeleteDialog
├── displays: sensor details
└── calls: onConfirm, onCancel callbacks
```

## 🔧 Customization Guide

### Change Items Per Page
In `SensorCRUDPage.jsx`:
```javascript
const [itemsPerPage] = useState(10);  // Change 10 to desired number
```

### Add New Category
In `SensorForm.jsx`:
```javascript
<option value="YourCategory">Your Category</option>
```

### Change Colors
In `SensorListTable.jsx`:
```javascript
const getCategoryColor = (category) => {
  // Modify the colors object
};
```

### Add New Field
1. Add to form in `SensorForm.jsx`
2. Add to table in `SensorListTable.jsx`
3. Add to API payload in `SensorCRUDPage.jsx`
4. Ensure backend Sensor model has field

## 📈 Performance Tips

1. **Search**: Real-time, no API calls (client-side)
2. **Pagination**: Reduces DOM elements on page
3. **Lazy Loading**: Sensors loaded once on mount
4. **Debouncing**: Prevents excessive re-renders
5. **Memoization**: Components optimized with React.memo (optional)

## 🧪 Test Data

### Test Sensor 1
- MAC: `00:1A:2B:3C:4D:5E`
- Node ID: `ENV-01`
- Category: `Environmental`
- Zone: `North Plot`
- Room: `Greenhouse`

### Test Sensor 2
- MAC: `AA:BB:CC:DD:EE:FF`
- Node ID: `PWR-02`
- Category: `Power`
- Zone: `Utility Wing`
- Room: `Generator Room`

## 📞 Support Files

- **Full Documentation**: `SENSOR_CRUD_GUIDE.md`
- **Implementation Details**: `SENSOR_CRUD_IMPLEMENTATION.md`
- **This File**: `SENSOR_CRUD_QUICKREF.md`

## ✨ Features Highlight

✅ **Complete CRUD Operations**
- Create, Read, Update, Delete sensors
- Full form validation
- Error handling with notifications

✅ **Search & Filter**
- Search across multiple fields
- Real-time filtering
- Instant results

✅ **Pagination**
- 10 items per page
- Smart page navigation
- Shows item count

✅ **User Experience**
- Clean, modern UI
- Dark mode support
- Mobile responsive
- Accessible design

✅ **Developer Friendly**
- Well-commented code
- Reusable components
- Easy to customize
- Clear file structure

---

**Status**: Ready to Use ✅  
**Last Updated**: August 20, 2026  
**Version**: 1.0.0
