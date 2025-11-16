# Requirements Document

## Introduction

This feature enhancement modernizes the existing EEE Routine Manager single-file HTML application by consolidating data fields, improving the user interface, enhancing export capabilities, and streamlining the overall user experience. The goal is to create a more efficient, professional-looking, and user-friendly routine management system while maintaining the single-file architecture and localStorage functionality.

## Requirements

### Requirement 1

**User Story:** As a user managing class schedules, I want time information displayed as a single combined field, so that I can quickly see the full time range without scanning multiple columns.

#### Acceptance Criteria

1. WHEN viewing the routine table THEN the system SHALL display time as a single field in format "8:20–9:50"
2. WHEN adding or editing a row THEN the system SHALL provide a single time input field that accepts formats like "8:20-9:50" or "8:20–9:50"
3. WHEN importing data THEN the system SHALL parse both separate start/end times and combined time formats
4. WHEN exporting data THEN the system SHALL output the combined time format
5. WHEN filtering by time THEN the system SHALL support filtering by start time, end time, or time range

### Requirement 2

**User Story:** As a user organizing academic schedules, I want level and term information combined into one field, so that I can see the complete academic context at a glance.

#### Acceptance Criteria

1. WHEN viewing the routine table THEN the system SHALL display level and term as "L1–T1" format
2. WHEN adding or editing a row THEN the system SHALL provide a single input that accepts "1-2", "L1-T2", or "L1–T1" formats
3. WHEN importing existing data THEN the system SHALL automatically combine separate level and term fields
4. WHEN exporting data THEN the system SHALL output the combined "L1–T1" format
5. WHEN filtering THEN the system SHALL allow filtering by the combined level-term field

### Requirement 3

**User Story:** As a user who needs to share routine data, I want multiple export options with customizable columns and professional branding, so that I can create reports tailored to different audiences and formats.

#### Acceptance Criteria

1. WHEN clicking export options THEN the system SHALL provide Excel (.xlsx) and PDF export buttons (CSV removed)
2. WHEN initiating any export THEN the system SHALL show a column selection dialog with checkboxes
3. WHEN selecting columns for export THEN the system SHALL only include checked columns in the output
4. WHEN exporting to Excel THEN the system SHALL create a properly formatted .xlsx file with headers and data
5. WHEN exporting to PDF THEN the system SHALL display "DIU EEE Routine" as the title with DIU logo
6. WHEN exporting to PDF THEN the system SHALL provide option to export in color or black & white
7. WHEN exporting to PDF THEN the system SHALL include date/time of export in the footer
8. WHEN exporting to PDF THEN the system SHALL use a clean tabular layout matching the app's theme
9. WHEN exporting filtered data THEN the system SHALL only export currently visible/filtered rows

### Requirement 4

**User Story:** As a user working with DIU routine Excel files, I want to import complex multi-sheet Excel files with automatic parsing, so that I can quickly load semester schedules without manual data entry.

#### Acceptance Criteria

1. WHEN importing Excel files THEN the system SHALL parse the specific DIU routine format (Routine_version_4 structure)
2. WHEN parsing Excel sheets THEN the system SHALL extract room numbers from Column B (rows 4-16 for theory, rows 20-31 and 34-35 for lab)
3. WHEN parsing theory classes THEN the system SHALL read slot headers from Row 2 and data from rows 4-16
4. WHEN parsing lab classes THEN the system SHALL read slot headers from Row 18 and data from rows 20-31 and 34-35
5. WHEN parsing course data THEN the system SHALL extract Level-Term from 1st column, Course Code from 2nd column (removing last letter for section), and Teacher from 3rd column
6. WHEN parsing section data THEN the system SHALL extract the last letter from course-section cell as the section
7. WHEN processing multiple sheets THEN the system SHALL treat each sheet as a different weekday and combine all data
8. WHEN encountering empty cells THEN the system SHALL skip them and continue processing
9. WHEN importing fails THEN the system SHALL provide clear error messages about format issues

### Requirement 5

**User Story:** As a user who wants a professional-looking application, I want a modern EEE-themed interface that works well on all devices, so that I can use the tool efficiently regardless of my device or screen size.

#### Acceptance Criteria

1. WHEN loading the application THEN the system SHALL display an Electrical & Electronic Engineering themed color palette with navy blue, cyan, silver, and electric green accents
2. WHEN using on mobile devices THEN the system SHALL provide a responsive layout that adapts to screen size
3. WHEN interacting with buttons and controls THEN the system SHALL provide smooth animations and hover transitions
4. WHEN viewing cards and panels THEN the system SHALL display rounded corners, soft shadows, and elegant typography (Inter, Poppins, or Roboto)
5. WHEN using the interface THEN the system SHALL maintain accessibility standards with proper contrast and focus indicators
6. WHEN viewing on different screen sizes THEN the system SHALL reorganize layout elements appropriately
7. WHEN viewing the interface THEN the system SHALL present a dynamic, professional, and modern look with improved spacing and layout hierarchy

### Requirement 6

**User Story:** As a user who values simplicity, I want CSV import/export removed and streamlined import options, so that the interface focuses on Excel and JSON workflows.

#### Acceptance Criteria

1. WHEN loading the application THEN the system SHALL NOT display CSV import/export buttons
2. WHEN viewing import options THEN the system SHALL only show Excel import and JSON restore buttons
3. WHEN viewing export options THEN the system SHALL only show Excel export, PDF export, and JSON backup buttons
4. WHEN using the application THEN the system SHALL have a cleaner interface focused on primary workflows

### Requirement 7

**User Story:** As a user who relies on data persistence, I want all new features to work seamlessly with the existing localStorage system, so that my data remains safe and accessible across browser sessions.

#### Acceptance Criteria

1. WHEN data is modified THEN the system SHALL automatically save to localStorage using the new combined field format
2. WHEN loading the application THEN the system SHALL migrate existing localStorage data to the new format if needed
3. WHEN backing up data THEN the system SHALL export JSON in the new combined format
4. WHEN restoring data THEN the system SHALL handle both old and new JSON formats
5. WHEN clearing data THEN the system SHALL maintain the same localStorage clearing functionality

### Requirement 8

**User Story:** As a DIU user, I want the application to display DIU branding and identity, so that exported documents and the interface reflect the institution.

#### Acceptance Criteria

1. WHEN exporting to PDF THEN the system SHALL include the DIU logo (top-left or centered)
2. WHEN exporting to PDF THEN the system SHALL display "DIU EEE Routine" as the document title
3. WHEN viewing the application THEN the system SHALL incorporate DIU branding elements in the header
4. WHEN exporting documents THEN the system SHALL maintain professional DIU identity throughout

### Requirement 9

**User Story:** As a user working with the single-file application, I want all improvements implemented with minimal external dependencies, so that the app remains portable and self-contained.

#### Acceptance Criteria

1. WHEN viewing the source THEN the system SHALL contain all CSS and JavaScript inline
2. WHEN using the application THEN the system SHALL work offline after initial load (except for CDN libraries)
3. WHEN deploying the application THEN the system SHALL work as a single HTML file
4. WHEN using export features THEN the system SHALL use SheetJS (xlsx.js) for Excel and jsPDF for PDF generation
5. WHEN styling the interface THEN the system SHALL use custom CSS without external frameworks