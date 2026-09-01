# Sensor CRUD Pages - Implementation Guide

## Overview

A complete CRUD (Create, Read, Update, Delete) interface for managing IoT sensors has been added to your Smart-X platform. This allows users to manage sensors through a user-friendly web interface integrated with your backend API.

## Features

### 1. **Sensor List View**
- Display all sensors in a responsive table
- Search/filter by MAC address, Node ID, Category, Zone, or Room
- Pagination with customizable items per page (10 by default)
- Status indicators (Active/Inactive)
- Category badges with color coding
- Quick action buttons (View, Edit, Delete)

### 2. **Create New Sensor**
- Form-based sensor creation
- MAC address validation (format: 00:1A:2B:3C:4D:5E)
- Required fields: MAC Address, Node ID, Category, Zone
- Optional fields: Room, Description
- Active status enabled by default
- Real-time form validation with error messages

### 3. **Edit Sensor**
- Inline edit form for existing sensors
- Pre-populated fields with current sensor data
- MAC address is read-only (cannot be changed)
- Validation on all field changes
- Save or cancel changes

### 4. **Delete Sensor**
- Confirmation dialog with sensor details
- Warning about cascading deletes (telemetry, alerts, commands)
- Safe deletion with error handling
- Auto-refresh of sensor list after deletion

### 5. **View Sensor Details**
- Link to detailed sensor page
- Shows all sensor information
- Displays telemetry data
- Shows related alerts and commands

## File Structure

```
src/
├── pages/
│   └── SensorCRUDPage.jsx          # Main CRUD page component
├── components/
│   ├── SensorForm.jsx              # Reusable form for create/edit
│   ├── SensorListTable.jsx         # Sensor list table component
│   └── SensorDeleteDialog.jsx      # Delete confirmation dialog
└── services/
	└── apiClient.js                 # API client (already configured)
```

## Component Details

### SensorCRUDPage.jsx
**Main page component that orchestrates all CRUD operations**

- Manages state for sensors, search, pagination, and dialogs
- Fetches sensors from API on mount
- Handles search/filter logic
- Coordinates form submission and deletion
- Provides loading and empty states
- Implements pagination with intelligent page number selection

**Key Features:**
- Automatic list refresh after create/update/delete
- Search across multiple fields
- Error handling with toast notifications
- Responsive layout for mobile and desktop

### SensorForm.jsx
**Reusable form component for creating and editing sensors**

- Auto-detects create vs. edit mode from props
- Real-time field validation
- Category dropdown with common sensor types
- MAC address format validation
- Active status checkbox
- Loading state during submission
- Cancel button to close form

**Fields:**
- MAC Address (required, disabled when editing)
- Node ID (required)
- Category (required dropdown)
- Zone (required)
- Room (optional)
- Active Status (checkbox)

### SensorListTable.jsx
**Displays sensors in a responsive table**

- Sortable columns (clickable headers)
- Status indicators with icons
- Category badges with colors
- Action buttons (View, Edit, Delete)
- Hover effects
- Dark mode support
- Responsive design

**Categories & Colors:**
- Environmental: Blue
- Power: Yellow
- Actuator: Purple
- Temperature: Orange
- Humidity: Cyan
- Other: Gray

### SensorDeleteDialog.jsx
**Modal dialog for confirming sensor deletion**

- Shows sensor details in dialog
- Warning about cascading deletes
- Confirmation button (Delete) and cancel button
- Loading state during deletion
- Accessible design with proper focus management

## API Integration

The component uses these API endpoints:

```javascript
// Get all sensors
GET /api/sensor
Response: Array of sensor objects

// Get single sensor
GET /api/sensor/{macaddress}
Response: Single sensor object with deployment info

// Create sensor
POST /api/sensor
Body: { macaddress, nodeId, category, zone, room?, isActive }
Response: Created sensor object

// Update sensor
PUT /api/sensor/{macaddress}
Body: { nodeId, category, zone, room?, isActive }
Response: Updated sensor object

// Delete sensor
DELETE /api/sensor/{macaddress}
Response: 204 No Content
```

## Routing

The new route has been added to `App.jsx`:

```javascript
<Route path="/sensors" element={<SensorCRUDPage />} />
```

Navigation is available via the Sidebar under "Sensor Management" section.

## Usage

### Access the Sensor CRUD Page
1. Click "Sensor Management" in the sidebar
2. Or navigate to `/sensors` directly

### Create a New Sensor
1. Click the "Add Sensor" button
2. Fill in all required fields
3. Optionally add room name
4. Check/uncheck "Active" status
5. Click "Create Sensor"

### Edit a Sensor
1. Find the sensor in the list
2. Click the edit icon (pencil)
3. Modify the fields (except MAC address)
4. Click "Update Sensor"

### Delete a Sensor
1. Find the sensor in the list
2. Click the delete icon (trash)
3. Review the confirmation dialog
4. Click "Delete Sensor" to confirm

### Search Sensors
1. Use the search bar at the top
2. Type any partial text to filter:
   - MAC address (00:1A:2B...)
   - Node ID (ENV-01, PWR-02)
   - Category (Environmental, Power)
   - Zone (North Plot, Utility Wing)
   - Room (Greenhouse, Generator Room)

### Pagination
- Use arrow buttons to navigate pages
- Click page numbers to jump to specific page
- Shows 10 sensors per page by default

## Styling

All components use:
- **Tailwind CSS** for styling
- **Lucide React** icons
- **Dark mode support** (automatic based on system preference)
- **Responsive design** (mobile, tablet, desktop)
- **Accessible color contrast** (WCAG compliant)

## Validation Rules

### MAC Address
- Format: `XX:XX:XX:XX:XX:XX` or `XX-XX-XX-XX-XX-XX`
- Example: `00:1A:2B:3C:4D:5E`
- Case-insensitive (automatically converted to uppercase)

### Node ID
- Any alphanumeric string
- Examples: `ENV-01`, `PWR-02`, `SENSOR-001`

### Category
- Dropdown selection
- Options: Environmental, Power, Actuator, Temperature, Humidity, Pressure, Light, Motion, Other

### Zone
- Required text field
- Examples: `North Plot`, `Utility Wing`, `Greenhouse`

### Room
- Optional text field
- Examples: `Greenhouse`, `Generator Room`

## Error Handling

- Network errors display toast notifications
- Validation errors show inline error messages
- Delete operations show confirmation dialog
- Loading states prevent double-submission
- Graceful fallbacks for missing data

## Performance

- Client-side search and filtering (no extra API calls)
- Debounced search (instant results)
- Pagination reduces DOM elements
- Lazy loading of sensor data
- Efficient re-renders with React hooks

## Accessibility

- Keyboard navigation support
- ARIA labels on buttons
- Semantic HTML structure
- High contrast colors
- Focus indicators on interactive elements
- Screen reader friendly

## Future Enhancements

Potential improvements:
1. Batch operations (select multiple sensors)
2. Export to CSV/JSON
3. Bulk import from file
4. Sorting by clicking headers
5. Advanced filters (date range, status)
6. Sensor groups/categories
7. Tags and custom attributes
8. Deployment hierarchy visualization
9. Real-time sync with WebSocket
10. Sensor health metrics

## Browser Support

- Chrome/Chromium 90+
- Firefox 88+
- Safari 14+
- Edge 90+

## Dependencies

- React 19.2.8+
- React Router DOM 6.20.0+
- Axios 1.6.2+
- Lucide React 0.344.0+
- Tailwind CSS 3.3.6+

## Troubleshooting

### "Failed to fetch sensors"
- Check backend API is running on `localhost:5231`
- Verify CORS is configured correctly
- Check network tab in browser dev tools

### Form validation errors
- MAC address must be in valid format
- All required fields must be filled
- Check for typos in required fields

### Delete fails
- Sensor might not exist
- Check backend permissions
- Verify sensor MAC address is correct

### Search not working
- Ensure sensor data is loaded
- Try searching for exact field values
- Clear search to reset list

## Support & Questions

For issues or questions about the Sensor CRUD implementation:
1. Check this documentation
2. Review component comments
3. Check browser console for errors
4. Verify backend API connectivity
5. Check network tab in browser dev tools
