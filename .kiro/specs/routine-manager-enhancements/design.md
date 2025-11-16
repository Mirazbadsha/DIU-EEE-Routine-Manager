# Design Document

## Overview

This design document outlines the technical approach for enhancing the DIU EEE Routine Manager with advanced filtering, empty room discovery, teacher information integration, and visual improvements. The solution builds upon the existing single-file HTML architecture while maintaining backward compatibility with the current data structure.

## Architecture

### High-Level Architecture

The application maintains its single-file HTML architecture with embedded JavaScript. The enhancements follow the existing patterns:

1. **Data Layer**: Extends current localStorage-based persistence to include teacher information mapping
2. **Filter System**: Enhances existing filter mechanism with keyword search and additional filter fields
3. **Room Finder Module**: New component that analyzes schedule data to identify available rooms
4. **Teacher Info Module**: New component that manages teacher data and provides interactive UI elements
5. **UI Layer**: Updates to existing CSS variables and component styles for improved readability

### Data Flow

```
Excel Import → Parse Routine Data + Teacher Data → Store in Memory
                                                    ↓
User Interaction → Filter/Search → Process Data → Update UI
                                                    ↓
                                    Empty Room Analysis → Display Results
                                                    ↓
                                    Teacher Click → Show Modal → Contact Actions
```

## Components and Interfaces

### 1. Enhanced Filter System

#### Filter Configuration Extension

```javascript
const COLUMNS = [
    // ... existing columns ...
    { key: 'classCode', label: 'Class Code', type: 'text', placeholder: 'EEE-101' },
    { key: 'courseTitle', label: 'Course Title', type: 'text', placeholder: 'Electrical Circuits' },
    // ... other columns ...
];
```

#### Hybrid Filter Component

Each filter field will support both dropdown and keyword search:

- **Dropdown Mode**: Shows unique values from dataset (existing behavior)
- **Keyword Search Mode**: Allows partial text matching with real-time filtering
- **Implementation**: Use `<datalist>` element or custom combo-box pattern

```html
<div class="filter-field">
    <label>Class Code</label>
    <input type="text" list="classCodeOptions" data-filter="classCode" placeholder="Type or select...">
    <datalist id="classCodeOptions">
        <!-- Dynamically populated options -->
    </datalist>
</div>
```

#### Filter Logic Enhancement

```javascript
function applyFiltersAndSort() {
    filteredRows = rows.filter(row => {
        return Object.entries(filters).every(([key, value]) => {
            if (!value) return true;
            const rowValue = (row[key] || '').toString().toLowerCase();
            // Support partial matching for keyword search
            return rowValue.includes(value.toLowerCase());
        });
    });
    // ... existing sort logic ...
}
```

### 2. Empty Room Finder Component

#### Room Availability Analysis

```javascript
const RoomFinder = {
    // Get all unique rooms from schedule
    getAllRooms() {
        return [...new Set(rows.map(row => row.room).filter(Boolean))].sort();
    },
    
    // Find rooms not scheduled for given criteria
    findEmptyRooms(day, timeSlot) {
        const allRooms = this.getAllRooms();
        const occupiedRooms = rows
            .filter(row => {
                let matches = true;
                if (day) matches = matches && row.day === day;
                if (timeSlot) matches = matches && row.time === timeSlot;
                return matches;
            })
            .map(row => row.room);
        
        return allRooms.filter(room => !occupiedRooms.includes(room));
    },
    
    // Get current filter state
    getCurrentFilters() {
        return {
            day: filters.day || null,
            timeSlot: filters.time || null
        };
    }
};
```

#### UI Component

```html
<div class="empty-room-finder">
    <h3>Available Rooms</h3>
    <div id="emptyRoomsList" class="room-list">
        <!-- Dynamically populated -->
    </div>
</div>
```

#### Display Logic

```javascript
function updateEmptyRooms() {
    const { day, timeSlot } = RoomFinder.getCurrentFilters();
    const emptyRoomsContainer = $('#emptyRoomsList');
    
    if (!day && !timeSlot) {
        emptyRoomsContainer.innerHTML = '<p class="hint">Select a day or time slot to find available rooms</p>';
        return;
    }
    
    const emptyRooms = RoomFinder.findEmptyRooms(day, timeSlot);
    
    if (emptyRooms.length === 0) {
        emptyRoomsContainer.innerHTML = '<p class="no-rooms">All rooms are occupied</p>';
        return;
    }
    
    const roomsHTML = emptyRooms.map(room => 
        `<span class="room-badge available">${room}</span>`
    ).join('');
    
    emptyRoomsContainer.innerHTML = roomsHTML;
}
```

### 3. Teacher Information Integration

#### Data Structure

```javascript
const teacherData = {
    // Map of initial to teacher details
    'RS': {
        name: 'Dr. M. Shamsul Alam',
        initial: 'RS',
        phone: '01713109917',
        email: 'eeesa@daffodilvarsity.edu.bd'
    },
    // ... more teachers
};
```

#### Excel Parser for Teacher Info

```javascript
function parseTeacherInfoExcel(workbook) {
    const sheet = workbook.Sheets[workbook.SheetNames[0]];
    const data = XLSX.utils.sheet_to_json(sheet);
    
    const teacherMap = {};
    data.forEach(row => {
        if (row.Initial && row.Name) {
            teacherMap[row.Initial.trim().toUpperCase()] = {
                name: row.Name || '',
                initial: row.Initial.trim().toUpperCase(),
                phone: row['Contact No.'] || '',
                email: row.Email || ''
            };
        }
    });
    
    return teacherMap;
}
```

#### Clickable Teacher Initial Component

```javascript
function renderTeacherCell(teacherInitial) {
    const teacher = teacherData[teacherInitial];
    
    if (teacher) {
        return `<a href="#" class="teacher-link" data-initial="${teacherInitial}">${teacherInitial}</a>`;
    }
    
    return teacherInitial; // Plain text if no data available
}
```

#### Teacher Details Modal

```html
<div class="modal-overlay hidden" id="teacherModal">
    <div class="modal teacher-modal">
        <div class="modal-header">
            <h3 id="teacherModalTitle">Teacher Details</h3>
            <button class="close-btn" id="closeTeacherModal">×</button>
        </div>
        <div class="modal-body">
            <div class="teacher-details">
                <div class="detail-row">
                    <span class="detail-label">Name:</span>
                    <span class="detail-value" id="teacherName"></span>
                </div>
                <div class="detail-row">
                    <span class="detail-label">Phone:</span>
                    <a href="#" class="detail-value contact-link" id="teacherPhone"></a>
                </div>
                <div class="detail-row">
                    <span class="detail-label">Email:</span>
                    <a href="#" class="detail-value contact-link" id="teacherEmail"></a>
                </div>
            </div>
        </div>
    </div>
</div>
```

#### Modal Interaction Logic

```javascript
function showTeacherModal(initial) {
    const teacher = teacherData[initial];
    if (!teacher) return;
    
    $('#teacherName').textContent = teacher.name;
    
    const phoneLink = $('#teacherPhone');
    phoneLink.textContent = teacher.phone;
    phoneLink.href = `tel:${teacher.phone}`;
    
    const emailLink = $('#teacherEmail');
    emailLink.textContent = teacher.email;
    emailLink.href = `mailto:${teacher.email}`;
    
    $('#teacherModal').classList.remove('hidden');
}

// Event delegation for teacher links
document.addEventListener('click', (e) => {
    if (e.target.classList.contains('teacher-link')) {
        e.preventDefault();
        const initial = e.target.dataset.initial;
        showTeacherModal(initial);
    }
});
```

### 4. Visual Design Improvements

#### Color Palette Refinement

Update CSS variables for better contrast and readability:

```css
:root {
    /* Improved text colors with better contrast */
    --text-primary: #ffffff;          /* Pure white for primary text */
    --text-secondary: #e2e8f0;        /* Lighter secondary text */
    --text-muted: #a0aec0;            /* Improved muted text */
    
    /* Enhanced accent colors */
    --accent-cyan: #22d3ee;           /* Brighter cyan */
    --accent-green: #34d399;          /* Brighter green */
    
    /* Better border visibility */
    --border-primary: #475569;        /* More visible borders */
}
```

#### Typography Improvements

```css
body {
    font-family: 'Inter', -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, sans-serif;
    -webkit-font-smoothing: antialiased;
    -moz-osx-font-smoothing: grayscale;
}

/* Improved table text */
tbody td {
    color: var(--text-primary);
    font-weight: 400;
}

/* Better label visibility */
.filter-field label,
.form-field label {
    color: var(--text-secondary);
    font-weight: 600;
}

/* Enhanced header gradient */
.header-content h1 {
    background: linear-gradient(135deg, #22d3ee, #34d399);
    -webkit-background-clip: text;
    -webkit-text-fill-color: transparent;
    filter: brightness(1.1);
}
```

## Data Models

### Teacher Information Model

```typescript
interface Teacher {
    name: string;        // Full name
    initial: string;     // Teacher initial/code
    phone: string;       // Contact number
    email: string;       // Email address
}

interface TeacherDataStore {
    [initial: string]: Teacher;
}
```

### Empty Room Result Model

```typescript
interface EmptyRoomQuery {
    day?: string;
    timeSlot?: string;
}

interface EmptyRoomResult {
    rooms: string[];
    query: EmptyRoomQuery;
    timestamp: Date;
}
```

## Error Handling

### Teacher Data Import Errors

1. **Missing Teacher Info File**: Display warning message, continue with routine functionality
2. **Invalid Excel Format**: Log error, show user-friendly message
3. **Missing Required Columns**: Skip invalid rows, import valid data

```javascript
function handleTeacherImportError(error) {
    console.error('Teacher import error:', error);
    updateStatus('Teacher info import failed. Routine features still available.', 'warning');
}
```

### Empty Room Finder Edge Cases

1. **No Rooms in Schedule**: Display "No rooms found in schedule"
2. **All Rooms Occupied**: Display "All rooms are occupied for selected criteria"
3. **Invalid Filter Combination**: Clear invalid filters, show hint message

### Teacher Modal Errors

1. **Missing Teacher Data**: Don't show link, display initial as plain text
2. **Invalid Contact Info**: Show field but disable link functionality

## Testing Strategy

### Unit Testing Approach

1. **Filter System Tests**
   - Test keyword search with partial matches
   - Test dropdown population with unique values
   - Test filter combination logic
   - Test filter chip display and removal

2. **Empty Room Finder Tests**
   - Test room extraction from schedule
   - Test availability calculation with day filter only
   - Test availability calculation with time slot filter only
   - Test availability calculation with both filters
   - Test edge cases (no rooms, all occupied)

3. **Teacher Info Tests**
   - Test Excel parsing with valid data
   - Test Excel parsing with missing columns
   - Test teacher lookup by initial
   - Test modal display with complete data
   - Test modal display with missing fields

4. **Visual Design Tests**
   - Test contrast ratios meet WCAG AA standards
   - Test text readability on all backgrounds
   - Test responsive behavior on mobile devices

### Integration Testing

1. **End-to-End Filter Flow**
   - Import routine → Apply filters → Verify results → Check empty rooms

2. **Teacher Info Flow**
   - Import teacher data → Import routine → Click teacher → Verify modal → Test contact links

3. **Combined Features**
   - Apply filters → Check empty rooms → Click teacher → Verify all features work together

### Manual Testing Checklist

- [ ] Import DIU Excel routine file
- [ ] Import teacher info Excel file
- [ ] Test each filter field with keyword search
- [ ] Test empty room finder with various filter combinations
- [ ] Click multiple teacher initials and verify modal data
- [ ] Test email and phone links from teacher modal
- [ ] Verify visual improvements on different screen sizes
- [ ] Test with missing teacher data
- [ ] Test with incomplete schedule data
- [ ] Verify localStorage persistence works with new features

## Performance Considerations

1. **Filter Performance**: Use debouncing for keyword search to avoid excessive re-renders
2. **Room Finder Caching**: Cache room availability results for current filter state
3. **Teacher Data Loading**: Load teacher data once on import, store in memory
4. **Modal Rendering**: Use event delegation to avoid attaching listeners to every teacher link

## Browser Compatibility

- Modern browsers (Chrome, Firefox, Safari, Edge) - last 2 versions
- Mobile browsers (iOS Safari, Chrome Mobile)
- Requires ES6+ support for arrow functions, template literals, and spread operator
- Uses CSS Grid and Flexbox (widely supported)

## Accessibility Considerations

1. **Keyboard Navigation**: Ensure all interactive elements are keyboard accessible
2. **Screen Reader Support**: Add ARIA labels to filter fields and buttons
3. **Color Contrast**: Maintain WCAG AA compliance (4.5:1 for normal text)
4. **Focus Indicators**: Ensure visible focus states for all interactive elements
5. **Modal Accessibility**: Trap focus within modal, support ESC key to close
