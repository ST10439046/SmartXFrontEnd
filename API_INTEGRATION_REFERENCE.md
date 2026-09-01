# API Integration Reference Guide

## Base Configuration

**Base URL**: `http://localhost:5231`
**Frontend Base URL**: `http://localhost:59760`
**Request Timeout**: Default (Axios)
**Response Format**: JSON

All requests are logged in browser console:
```
API Request: GET /api/sensor
API Response: 200 [{...}]
```

---

## 1. SENSOR ENDPOINTS

### GET /api/sensor - Get All Sensors
**Frontend Usage**: `TelemetryDashboard.jsx` on load and every 10 seconds
```javascript
const response = await sensorApi.getAllSensors();
// response.data = [{...}, {...}, ...]
```

**Expected Response Shape**:
```json
[
  {
	"macaddress": "00:1A:2B:3C:4D:5E",
	"sensorName": "Door Sensor 1",
	"room": "Front Entrance",
	"zone": "Front Door",
	"location": "Main Door",
	"nodeId": "1",
	"category": "power_consumption",
	"isActive": true,
	"registeredAt": "2026-08-16T15:36:20.650Z",
	"activeAlerts": [],
	"attachments": [],
	"commandHistories": [],
	"networkTopologyFromNodeMacNavigations": [],
	"networkTopologyToNodeMacNavigations": [],
	"telemetryData": []
  }
]
```

**UI Field Mappings**:
```javascript
sensor.sensorName        → Card Title
sensor.macAddress        → Identifier (MAC)
sensor.location          → Location display
sensor.category          → Category tag
sensor.isActive          → Online/Offline status
sensor.zone              → Zone info
sensor.registeredAt      → Registration date
```

---

### POST /api/sensor - Create New Sensor
**Frontend Usage**: `SensorRegistrationForm.jsx` on form submit
```javascript
const sensorData = {
  macAddress: "00:1A:2B:3C:4D:5E",  // REQUIRED - uppercase
  sensorName: "New Sensor",
  zone: "Office",
  nodeId: "2",
  category: "temperature",
  room: "Room 101",
  location: "Desk Corner",
  isActive: true
};

const response = await sensorApi.createSensor(sensorData);
// response.data = { macaddress: "00:1A:2B:3C:4D:5E", ... }
```

**Request Body Format**:
```json
{
  "macAddress": "00:1A:2B:3C:4D:5E",
  "sensorName": "New Sensor Name",
  "room": "Room 101",
  "zone": "Office",
  "location": "Wall Mount",
  "nodeId": "2",
  "category": "temperature",
  "isActive": true
}
```

**Validation Rules**:
- `macAddress`: REQUIRED, must be uppercase, format `XX:XX:XX:XX:XX:XX`
- `sensorName`: REQUIRED, string
- `zone`: REQUIRED, string
- `nodeId`: REQUIRED, string
- `category`: REQUIRED, one of: `temperature`, `humidity`, `power_consumption`, `motion`, `light`, `pressure`
- `room`: Optional, string
- `location`: Optional, string

**Success Response**:
```json
{
  "macaddress": "00:1A:2B:3C:4D:5E",
  "sensorName": "New Sensor Name",
  "zone": "Office",
  // ... other fields
}
```

**Error Response**:
```json
{
  "errors": {
	"MacAddress": ["MAC address format is invalid"],
	"SensorName": ["Sensor Name is required"]
  },
  "type": "https://tools.ietf.org/html/rfc7231#section-6.5.1",
  "title": "One or more validation errors occurred.",
  "status": 400,
  "traceId": "..."
}
```

---

### GET /api/sensor/{macaddress} - Get Sensor Details
**Frontend Usage**: Optional - for individual sensor view
```javascript
const response = await sensorApi.getSensorByMac("00:1A:2B:3C:4D:5E");
// response.data = { macaddress: "00:1A:2B:3C:4D:5E", ... }
```

**Response**: Same shape as single sensor from GET all

---

### PUT /api/sensor/{macaddress} - Update Sensor
**Frontend Usage**: Optional - for sensor editing
```javascript
const updatedData = {
  sensorName: "Updated Name",
  zone: "New Zone",
  // ... other fields to update
};

const response = await sensorApi.updateSensor("00:1A:2B:3C:4D:5E", updatedData);
```

---

### DELETE /api/sensor/{macaddress} - Delete Sensor
**Frontend Usage**: Optional - for sensor removal
```javascript
const response = await sensorApi.deleteSensor("00:1A:2B:3C:4D:5E");
```

---

## 2. TELEMETRY DATA ENDPOINTS

### GET /api/telemetrydata - Get All Telemetry
**Frontend Usage**: `TelemetryDashboard.jsx` groups by `sensorMacAddress`
```javascript
const response = await telemetryApi.getAllTelemetry();
// response.data = [{...}, {...}, ...]
```

**Expected Response Shape**:
```json
[
  {
	"id": 1,
	"sensorMacAddress": "00:1A:2B:3C:4D:5E",
	"value": 42.5,
	"unit": "°C",
	"timestamp": "2026-08-16T15:45:30.123Z",
	"rawData": "{...raw sensor data...}",
	"dataType": "temperature"
  },
  {
	"id": 2,
	"sensorMacAddress": "00:1A:2B:3C:4D:5E",
	"value": 55.2,
	"unit": "°C",
	"timestamp": "2026-08-16T15:50:30.123Z",
	"rawData": "{...}",
	"dataType": "temperature"
  }
]
```

**UI Field Mappings**:
```javascript
telemetry.sensorMacAddress  → Link telemetry to sensor
telemetry.value             → Current reading display
telemetry.unit              → Unit suffix (°C, kW, %)
telemetry.timestamp         → Last updated time
telemetry.dataType          → Data category
```

**Dashboard Processing**:
```javascript
// Group telemetry by sensor MAC (latest reading per sensor)
const telemetryByMac = {};
telemetryArray.forEach((telemetry) => {
  if (telemetry.sensorMacAddress) {
	// Keep only the latest timestamp for each sensor
	if (!telemetryByMac[telemetry.sensorMacAddress] ||
		new Date(telemetry.timestamp) > new Date(telemetryByMac[telemetry.sensorMacAddress].timestamp)) {
	  telemetryByMac[telemetry.sensorMacAddress] = telemetry;
	}
  }
});
```

---

### POST /api/telemetrydata - Create Telemetry Record
**Frontend Usage**: Optional - for manual data submission
```javascript
const telemetryData = {
  sensorMacAddress: "00:1A:2B:3C:4D:5E",
  value: 42.5,
  unit: "°C",
  dataType: "temperature",
  rawData: "{...sensor readings...}"
};

const response = await telemetryApi.createTelemetry(telemetryData);
```

**Request Body Format**:
```json
{
  "sensorMacAddress": "00:1A:2B:3C:4D:5E",
  "value": 42.5,
  "unit": "°C",
  "dataType": "temperature",
  "rawData": "Optional raw JSON string",
  "timestamp": "2026-08-16T15:45:30.123Z" // Optional - server uses current time if omitted
}
```

---

## 3. ACTIVE ALERT ENDPOINTS

### GET /api/activealert - Get All Alerts
**Frontend Usage**: `TelemetryDashboard.jsx` displays top 5
```javascript
const response = await alertApi.getAllAlerts();
// response.data = [{...}, {...}, ...]
// Dashboard limits to first 5 alerts
```

**Expected Response Shape**:
```json
[
  {
	"id": 1,
	"sensorMacAddress": "00:1A:2B:3C:4D:5E",
	"alertMessage": "Temperature exceeds threshold",
	"alertDate": "2026-08-16T15:45:00.000Z",
	"severity": "warning",
	"isActive": true,
	"resolvedAt": null,
	"alertType": "threshold_breach"
  },
  {
	"id": 2,
	"sensorMacAddress": "00:1A:2B:3C:4D:5E",
	"alertMessage": "Sensor offline",
	"alertDate": "2026-08-16T16:00:00.000Z",
	"severity": "critical",
	"isActive": true,
	"resolvedAt": null,
	"alertType": "connectivity"
  }
]
```

**UI Field Mappings**:
```javascript
alert.alertMessage    → Main text display
alert.alertDate       → Timestamp (formatted to time)
alert.severity        → Color coding (info/warning/critical)
alert.id              → Unique identifier
alert.isActive        → Active status
alert.sensorMacAddress → Linked sensor
```

**Severity Color Mapping**:
```javascript
{
  "info": {
	bgColor: 'bg-primary-50 dark:bg-primary-900/20',
	borderColor: 'border-primary-300',
	textColor: 'text-primary-800',
	icon: 'ℹ️'
  },
  "warning": {
	bgColor: 'bg-warning-50 dark:bg-warning-900/20',
	borderColor: 'border-warning-300',
	textColor: 'text-warning-800',
	icon: '⚠️'
  },
  "critical": {
	bgColor: 'bg-danger-50 dark:bg-danger-900/20',
	borderColor: 'border-danger-300',
	textColor: 'text-danger-800',
	icon: '🚨'
  }
}
```

---

### POST /api/activealert - Create Alert
```javascript
const alertData = {
  sensorMacAddress: "00:1A:2B:3C:4D:5E",
  alertMessage: "Threshold exceeded",
  severity: "warning",
  alertType: "threshold_breach"
};

const response = await alertApi.createAlert(alertData);
```

---

## 4. ATTACHMENT ENDPOINTS

### GET /api/attachment - Get All Attachments
```javascript
const response = await attachmentApi.getAllAttachments();
// response.data = [{...}, {...}, ...]
```

**Expected Response Shape**:
```json
[
  {
	"id": 1,
	"sensorMacAddress": "00:1A:2B:3C:4D:5E",
	"fileName": "config.json",
	"fileType": "config",
	"fileSize": 2048,
	"uploadDate": "2026-08-16T15:36:00.000Z",
	"filePath": "/attachments/config_123.json"
  },
  {
	"id": 2,
	"sensorMacAddress": "00:1A:2B:3C:4D:5E",
	"fileName": "sensor_photo.jpg",
	"fileType": "photo",
	"fileSize": 102400,
	"uploadDate": "2026-08-16T15:37:00.000Z",
	"filePath": "/attachments/sensor_photo_456.jpg"
  }
]
```

---

### POST /api/attachment - Upload Attachment
**Frontend Usage**: `SensorRegistrationForm.jsx` for each selected file
```javascript
const formData = new FormData();
formData.append('file', fileObject);
formData.append('sensorMacAddress', "00:1A:2B:3C:4D:5E");
formData.append('fileType', 'photo'); // 'photo', 'config', 'log', 'document', 'other'
formData.append('uploadDate', new Date().toISOString());

const response = await attachmentApi.createAttachment(formData);
```

**Important**: When uploading attachments, do NOT set `Content-Type` header manually - axios will auto-detect and set it as `multipart/form-data`

**File Type Classification** (from `SensorRegistrationForm.jsx`):
```javascript
const getFileType = (filename) => {
  const ext = filename.split('.').pop().toLowerCase();
  if (['jpg', 'jpeg', 'png', 'gif'].includes(ext)) return 'photo';
  if (['json', 'xml', 'csv', 'conf'].includes(ext)) return 'config';
  if (['log', 'txt'].includes(ext)) return 'log';
  if (['pdf'].includes(ext)) return 'document';
  return 'other';
};
```

---

## 5. COMMAND HISTORY ENDPOINTS

### GET /api/commandhistory - Get All Commands
**Frontend Usage**: Ready for Commands pillar implementation
```javascript
const response = await commandApi.getAllCommands();
// response.data = [{...}, {...}, ...]
```

**Expected Response Shape**:
```json
[
  {
	"id": 1,
	"sensorMacAddress": "00:1A:2B:3C:4D:5E",
	"command": "RESET",
	"commandDate": "2026-08-16T15:45:00.000Z",
	"status": "executed",
	"response": "OK",
	"executionTime": 250
  }
]
```

---

### POST /api/commandhistory - Create Command
```javascript
const commandData = {
  sensorMacAddress: "00:1A:2B:3C:4D:5E",
  command: "RESET",
  commandDate: new Date().toISOString()
};

const response = await commandApi.createCommand(commandData);
```

---

## 6. NETWORK TOPOLOGY ENDPOINTS

### GET /api/networktopology - Get All Topologies
**Frontend Usage**: Ready for Topology pillar implementation
```javascript
const response = await topologyApi.getAllTopologies();
// response.data = [{...}, {...}, ...]
```

**Expected Response Shape**:
```json
[
  {
	"id": 1,
	"fromNodeMac": "00:1A:2B:3C:4D:5E",
	"toNodeMac": "00:2B:3C:4D:5E:6F",
	"linkType": "direct",
	"signalStrength": -65,
	"isActive": true
  }
]
```

---

## Error Handling

All API calls are wrapped in try-catch and logged:

```javascript
try {
  const response = await sensorApi.getAllSensors();
  // Success: response.data contains the data
} catch (error) {
  // Error: Check console for logs
  console.error('Error:', error.response?.status, error.response?.data);
  // Common status codes:
  // 400 - Validation error
  // 404 - Not found
  // 500 - Server error
}
```

**Components handle errors with toast notifications**:
```javascript
const { show } = useNotification();

try {
  await sensorApi.createSensor(data);
  show('Sensor created successfully', 'success');
} catch (error) {
  show('Failed to create sensor', 'error');
}
```

---

## CORS Configuration

**Currently**: Frontend runs on `http://localhost:59760`, Backend on `http://localhost:5231`

If moving to production, ensure backend CORS policy allows frontend origin:
```csharp
// In backend Startup.cs or Program.cs
builder.Services.AddCors(options =>
{
	options.AddPolicy("AllowFrontend", policy =>
	{
		policy
			.WithOrigins("http://localhost:59760", "https://yourdomain.com")
			.AllowAnyMethod()
			.AllowAnyHeader()
			.AllowCredentials();
	});
});
```

---

## Auto-Refresh Behavior

**TelemetryDashboard** automatically refreshes every 10 seconds:
```javascript
const interval = setInterval(fetchData, 10000); // 10,000ms = 10 seconds
return () => clearInterval(interval); // Cleanup on unmount
```

This keeps sensors, telemetry, and alerts automatically synced with the backend.

---

## Browser Console Logs

All API activity is logged for debugging:

```
API Request: GET /api/sensor
API Response: 200 [{...}]

API Request: POST /api/sensor
API Response: 201 {...}

API Request: GET /api/telemetrydata
API Response: 200 [{...}]

API Error: 400 {errors: {...}}
```

Check the **Network** tab in DevTools to inspect actual HTTP requests/responses.

---

## Troubleshooting

### Issue: 404 Not Found
**Cause**: API endpoint doesn't exist or macro address is wrong
**Solution**: Verify endpoint URL and parameters in console logs

### Issue: CORS Error
**Cause**: Backend not allowing requests from frontend origin
**Solution**: Configure CORS in backend API

### Issue: 400 Bad Request
**Cause**: Invalid request data
**Solution**: Check validation errors in response body and fix field values

### Issue: 500 Server Error
**Cause**: Backend error
**Solution**: Check backend logs and ensure .NET API is running correctly

### Issue: Timeout
**Cause**: Backend not responding or connection issue
**Solution**: Verify backend is running on `http://localhost:5231`

---

**Last Updated**: Integration Complete
**Version**: 1.0
