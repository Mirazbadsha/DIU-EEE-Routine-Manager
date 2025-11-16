# Design Document

## Overview

The modernized EEE Routine Manager will transform the existing single-file HTML application into a more efficient and professional tool. The design focuses on consolidating data fields, implementing a modern UI with improved UX patterns, and enhancing export capabilities while maintaining the self-contained architecture and localStorage persistence.

## Architecture

### Data Model Changes

**Combined Time Field:**
- Store time as single string: `"8:20–9:50"`
- Internal parsing functions to extract start/end for filtering and sorting
- Support multiple input formats: `"8:20-9:50"`, `"8:20–9:50"`, `"8:20 - 9:50"`

**Combined Level-Term Field:**
- Store as single string: `"L1–T1"` 
- Parse input formats: `"1-2"`, `"L1-T2"`, `"1–2"`, `"L1–T1"`
- Maintain backward compatibility with separate level/term data

**Updated Column Structure:**
```javascript
const COLUMNS = [
  { key: 'day', label: 'Day' },
  { key: 'type', label: 'Type' },
  { key: 'time', label: 'Time' },           // Combined start-end
  { key: 'room', label: 'Room' },
  { key: 'levelTerm', label: 'Level–Term' }, // Combined level-term
  { key: 'section', label: 'Section' },
  { key: 'classCode', label: 'Class Code' },
  { key: 'courseTitle', label: 'Course Title' },
  { key: 'teacher', label: 'Teacher Initial' },
  { key: 'notes', label: 'Notes' }
];
```

### UI/UX Design System

**Color Palette (EEE-Themed Dark Design):**
```css
:root {
  /* Backgrounds - Navy Blue Base */
  --bg-primary: #0a1628;        /* Deep navy */
  --bg-secondary: #0f1e3a;      /* Navy blue */
  --bg-tertiary: #1a2942;       /* Lighter navy */
  
  /* Surfaces - Layered depth */
  --surface-primary: #1e3a5f;   /* Navy surface */
  --surface-secondary: #2d4a6f; /* Elevated surface */
  --surface-tertiary: #3a5a7f;  /* Highest surface */
  
  /* Text - High contrast */
  --text-primary: #f0f4f8;      /* Almost white */
  --text-secondary: #cbd5e0;    /* Light gray */
  --text-muted: #94a3b8;        /* Muted gray */
  --text-disabled: #64748b;     /* Disabled gray */
  
  /* Accents - EEE Theme */
  --accent-cyan: #06b6d4;       /* Electric cyan */
  --accent-cyan-hover: #0891b2; /* Darker cyan */
  --accent-green: #10b981;      /* Electric green */
  --accent-green-hover: #059669;/* Darker green */
  --accent-silver: #94a3b8;     /* Silver/gray */
  --accent-gold: #fbbf24;       /* Warning/highlight */
  
  /* Status colors */
  --success: #10b981;
  --warning: #fbbf24;
  --danger: #ef4444;
  --info: #06b6d4;
  
  /* Borders and shadows */
  --border-primary: #334155;
  --border-secondary: #475569;
  --border-accent: #06b6d4;
  --shadow-sm: 0 1px 2px 0 rgba(0, 0, 0, 0.3);
  --shadow-md: 0 4px 6px -1px rgba(0, 0, 0, 0.4);
  --shadow-lg: 0 10px 15px -3px rgba(0, 0, 0, 0.5);
  --shadow-glow: 0 0 20px rgba(6, 182, 212, 0.3);
}
```

**Component Design Patterns:**
- **Cards**: Rounded corners (12px), subtle shadows, gradient backgrounds
- **Buttons**: Multiple variants (primary, secondary, ghost) with hover states
- **Inputs**: Consistent styling with focus rings and validation states
- **Tables**: Alternating row colors, sticky headers, hover effects
- **Modals**: Backdrop blur, centered positioning, smooth animations

## Components and Interfaces

### 1. Time Input Component
```javascript
class TimeInput {
  // Accepts: "8:20-9:50", "8:20–9:50", "8:20 - 9:50"
  // Outputs: "8:20–9:50"
  parse(input) { /* parsing logic */ }
  format(timeString) { /* formatting logic */ }
  validate(input) { /* validation logic */ }
}
```

### 2. Level-Term Input Component  
```javascript
class LevelTermInput {
  // Accepts: "1-2", "L1-T2", "1–2", "L1–T1"
  // Outputs: "L1–T1"
  parse(input) { /* parsing logic */ }
  format(levelTermString) { /* formatting logic */ }
  validate(input) { /* validation logic */ }
}
```

### 3. Export Manager
```javascript
class ExportManager {
  showColumnSelector(availableColumns, callback) { /* UI for column selection */ }
  exportToExcel(data, selectedColumns) { /* Excel export using XLSX */ }
  exportToPDF(data, selectedColumns, options) { 
    // options: { colorMode: 'color' | 'bw', includeLogo: true }
    // Uses jsPDF library
    // Includes DIU logo and "DIU EEE Routine" title
    // Adds export date/time in footer
  }
}
```

### 4. Data Migration Service
```javascript
class DataMigration {
  migrateFromOldFormat(oldData) {
    // Convert separate start/end to combined time
    // Convert separate level/term to combined levelTerm
    // Maintain all other fields
  }
  
  isOldFormat(data) { /* detection logic */ }
  
  backwardCompatibleImport(importedData) {
    // Handle both old and new formats during import
  }
}
```

### 5. DIU Excel Parser
```javascript
class DIUExcelParser {
  parseRoutineFile(workbook) {
    // Parse all sheets (each sheet = one weekday)
    const allClasses = [];
    
    workbook.SheetNames.forEach(sheetName => {
      const sheet = workbook.Sheets[sheetName];
      const day = this.extractDayFromSheetName(sheetName);
      
      // Parse theory classes (rows 4-16, column B for rooms)
      const theoryClasses = this.parseTheoryClasses(sheet, day);
      
      // Parse lab classes (rows 20-31, 34-35, column B for rooms)
      const labClasses = this.parseLabClasses(sheet, day);
      
      allClasses.push(...theoryClasses, ...labClasses);
    });
    
    return allClasses;
  }
  
  parseTheoryClasses(sheet, day) {
    // Row 2: slot headers (every 3 columns)
    // Rows 4-16: room in col B, then 3-col groups (L-T, Course-Section, Teacher)
    // Extract section from last char of course-section
    // Extract section from last char of L-T
  }
  
  parseLabClasses(sheet, day) {
    // Row 18: slot headers (every 3 columns)
    // Rows 20-31, 34-35: same structure as theory
  }
  
  extractSection(courseSection) {
    // Get last character as section
    return courseSection.slice(-1).toUpperCase();
  }
  
  extractCourseCode(courseSection) {
    // Remove last character (section)
    return courseSection.slice(0, -1).trim();
  }
}
```

### 6. Modern UI Components

**Header with DIU Branding:**
- DIU logo or text branding in header
- "DIU EEE Routine Manager" title with gradient effect
- Action buttons aligned to the right

**Enhanced Filter Panel:**
- Grouped filters in collapsible sections
- Real-time filter chips showing active filters
- Quick filter presets (e.g., "Today's Classes", "Labs Only")
- EEE-themed styling with cyan accents

**Improved Table:**
- Sticky header with navy background
- Hover effects with cyan glow
- Smooth animations on row interactions
- Badge styling for Theory/Lab with themed colors

**Modal System:**
- Backdrop blur effect
- Smooth slide-in animations
- Form validation with real-time feedback
- Keyboard navigation support
- EEE-themed buttons and inputs

**PDF Export Options Modal:**
- Radio buttons for color vs black & white selection
- Preview of export settings
- Column selection integrated

## Data Models

### Updated Row Schema
```javascript
const RowSchema = {
  day: String,           // "Monday", "Tuesday", etc.
  type: String,          // "Theory", "Lab"
  time: String,          // "8:20–9:50" (combined)
  room: String,          // "301", "Lab-A", etc.
  levelTerm: String,     // "L1–T1" (combined)
  section: String,       // "A", "B", etc.
  classCode: String,     // "0713-111"
  courseTitle: String,   // "Electrical Circuits I"
  teacher: String,       // "MRK", "DTF"
  notes: String          // Optional notes
};
```

### Migration Mapping
```javascript
const MigrationMap = {
  // Old format -> New format
  start: (row) => row.time?.split('–')[0] || row.start,
  end: (row) => row.time?.split('–')[1] || row.end,
  level: (row) => row.levelTerm?.match(/L(\d+)/)?.[1] || row.level,
  term: (row) => row.levelTerm?.match(/T(\d+)/)?.[1] || row.term,
  
  // New format <- Old format
  time: (row) => row.start && row.end ? `${row.start}–${row.end}` : row.time,
  levelTerm: (row) => row.level && row.term ? `L${row.level}–T${row.term}` : row.levelTerm
};
```

## Error Handling

### Import Error Handling
- **Invalid Time Format**: Show specific error with examples of valid formats
- **Missing Required Fields**: Highlight which fields are required
- **File Format Issues**: Provide clear guidance on supported formats
- **Data Type Mismatches**: Auto-convert when possible, warn when not

### Export Error Handling
- **No Data Selected**: Prompt user to add data or adjust filters
- **Column Selection**: Require at least one column for export
- **File Generation Failures**: Provide fallback options and error details

### UI Error States
- **Form Validation**: Real-time validation with helpful error messages
- **Network Issues**: Graceful handling of offline scenarios
- **Browser Compatibility**: Feature detection and fallbacks

## Testing Strategy

### Unit Testing Approach
```javascript
// Test data parsing and formatting
describe('TimeInput', () => {
  test('parses various time formats', () => {
    expect(TimeInput.parse('8:20-9:50')).toBe('8:20–9:50');
    expect(TimeInput.parse('8:20 - 9:50')).toBe('8:20–9:50');
  });
});

// Test data migration
describe('DataMigration', () => {
  test('migrates old format to new format', () => {
    const oldRow = { start: '8:20', end: '9:50', level: '1', term: '2' };
    const newRow = DataMigration.migrateFromOldFormat(oldRow);
    expect(newRow.time).toBe('8:20–9:50');
    expect(newRow.levelTerm).toBe('L1–T2');
  });
});
```

### Integration Testing
- **Import/Export Workflows**: Test complete data flow from import to export
- **Filter Combinations**: Test complex filter scenarios
- **Data Persistence**: Verify localStorage operations work correctly
- **Cross-Browser Compatibility**: Test in major browsers

### User Acceptance Testing
- **Data Migration**: Verify existing user data migrates correctly
- **Workflow Efficiency**: Measure time savings with combined fields
- **Mobile Usability**: Test responsive design on various devices
- **Export Quality**: Verify exported files meet user requirements

### Performance Testing
- **Large Datasets**: Test with 1000+ routine entries
- **Filter Performance**: Measure filter response times
- **Export Speed**: Benchmark export generation times
- **Memory Usage**: Monitor browser memory consumption

## DIU Logo Integration

**Logo Embedding Strategy:**
```javascript
// Embed DIU logo as base64 data URI for portability
const DIU_LOGO_BASE64 = 'data:image/png;base64,...';

// Use in PDF export
function addLogoToPDF(doc) {
  doc.addImage(DIU_LOGO_BASE64, 'PNG', 10, 10, 30, 30);
}

// Use in header (optional)
function renderHeader() {
  return `
    <div class="header-logo">
      <img src="${DIU_LOGO_BASE64}" alt="DIU Logo" />
    </div>
  `;
}
```

## PDF Export Design

**Layout Structure:**
```
┌─────────────────────────────────────┐
│  [DIU Logo]   DIU EEE Routine       │
│                                     │
│  ┌───────────────────────────────┐ │
│  │  Selected Columns Table       │ │
│  │  (Color or B&W based on option)│ │
│  └───────────────────────────────┘ │
│                                     │
│  Exported on: [Date & Time]         │
└─────────────────────────────────────┘
```

**Color vs B&W Modes:**
- **Color Mode**: Use full EEE theme colors (cyan, green, navy)
- **B&W Mode**: Convert to grayscale, use borders for distinction

## Implementation Phases

### Phase 1: Data Model Updates
- Implement combined time and level-term parsing
- Create data migration utilities
- Update localStorage schema

### Phase 2: UI Modernization  
- Implement EEE-themed design system (navy, cyan, silver, green)
- Update all components with modern styling and animations
- Remove CSV import/export functionality
- Add DIU branding to header

### Phase 3: DIU Excel Import Parser
- Implement DIUExcelParser class
- Add multi-sheet parsing logic
- Extract theory classes (rows 4-16)
- Extract lab classes (rows 20-31, 34-35)
- Parse 3-column slot structure
- Handle section extraction from course codes

### Phase 4: Enhanced Exports
- Add Excel export capability with column selection
- Implement PDF export using jsPDF library
- Add DIU logo and branding to PDF
- Implement color vs B&W export option
- Add export date/time footer
- Create column selection interface

### Phase 5: Polish & Testing
- Test DIU Excel import with actual files
- Verify PDF exports in both color modes
- Responsive design refinements
- Performance optimizations
- Cross-browser testing and fixes