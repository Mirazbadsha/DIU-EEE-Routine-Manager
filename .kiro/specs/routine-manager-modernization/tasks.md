# Implementation Plan

- [x] 1. Create data parsing and validation utilities


  - Implement time parsing functions that convert formats like "8:20-9:50" to "8:20–9:50"
  - Create level-term parsing functions that convert "1-2", "L1-T2" to "L1–T1" format
  - Add validation functions for combined field formats with helpful error messages
  - Create utility functions for extracting start/end times and level/term from combined fields
  - _Requirements: 1.2, 1.3, 2.2, 2.3_


- [x] 2. Implement data migration system for localStorage




  - Create migration functions to convert existing localStorage data from separate start/end/level/term to combined time/levelTerm fields
  - Add detection logic to identify old vs new data format in localStorage
  - Implement automatic migration on app load that preserves all existing user data
  - Add error handling and recovery for migration failures

  - _Requirements: 7.1, 7.2, 7.3, 7.4_

- [ ] 3. Update COLUMNS array and core data structure
  - Modify COLUMNS array to replace start/end with single 'time' field and level/term with single 'levelTerm' field
  - Update all references to old column keys throughout the codebase
  - Ensure backward compatibility for filtering and sorting operations

  - Update data normalization to work with new combined field structure
  - _Requirements: 1.1, 2.1, 8.4_

- [ ] 4. Remove CSV import/export functionality
  - Remove CSV import input element and event handlers from HTML
  - Remove CSV export button and related functions
  - Remove CSV template download functionality
  - Clean up CSV-related variables and functions from JavaScript
  - Reorganize layout to focus on Excel and JSON workflows
  - _Requirements: 6.1, 6.2, 6.3, 6.4_

- [ ] 5. Modernize CSS with EEE-themed design system
  - Replace existing CSS variables with EEE color palette (navy blue, cyan, silver, electric green)
  - Update component styling with modern rounded corners, soft shadows, and gradients
  - Implement smooth animations and hover transitions with cyan glow effects
  - Add elegant typography using Inter, Poppins, or Roboto font families
  - Implement improved button variants with EEE theme colors
  - Add responsive design improvements for mobile and tablet devices
  - Create consistent spacing, typography, and visual hierarchy throughout
  - _Requirements: 5.1, 5.3, 5.4, 5.5, 5.6, 5.7_

- [ ] 6. Update form inputs for combined fields
  - Replace separate start/end time inputs with single time input field

  - Replace separate level/term inputs with single level-term input field
  - Add placeholder text and input validation for new combined field formats
  - Update form submission logic to create combined field values
  - Implement real-time input formatting and validation feedback
  - _Requirements: 1.2, 2.2, 4.4_


- [ ] 7. Update table rendering for combined fields
  - Modify table header generation to show "Time" and "Level-Term" columns
  - Update row rendering to display combined fields with proper formatting
  - Ensure table sorting works correctly with new combined field formats
  - Add improved hover effects and visual styling for better user experience
  - _Requirements: 1.1, 2.1, 5.2_


- [ ] 8. Update filtering system for combined fields
  - Modify time filtering to work with combined time field (support start time, end time, or range filtering)
  - Update level-term filtering to work with combined "L1–T1" format
  - Enhance filter UI styling to match new design system
  - Ensure filter chips display correctly with new field names

  - _Requirements: 1.5, 2.5, 5.1_

- [ ] 9. Create column selection modal for exports
  - Implement modal dialog component with checkboxes for each available column
  - Add select all/none functionality and intuitive column organization
  - Style modal with modern UI patterns matching the new design system
  - Implement column selection state management for user preferences

  - _Requirements: 3.2, 3.3_

- [ ] 10. Add Excel export functionality with column selection
  - Implement Excel export using existing XLSX library with new combined field structure
  - Integrate with column selection modal to export only selected columns
  - Add proper Excel formatting with headers, data types, and cell styling
  - Include error handling for Excel generation failures and large datasets

  - Ensure export respects current filter state and only exports visible rows
  - _Requirements: 3.1, 3.4, 3.6_

- [ ] 11. Implement professional PDF export with DIU branding
  - Add jsPDF library via CDN to the HTML file
  - Create PDF export functionality using jsPDF with table plugin
  - Embed DIU logo as base64 data URI for portability
  - Add "DIU EEE Routine" title at the top of PDF with proper formatting
  - Implement color vs black & white export mode selection modal
  - Design PDF layout with DIU logo (top-left or centered), title, and table
  - Add export date/time in the footer of PDF
  - Integrate with column selection to only include selected columns in PDF
  - Apply EEE theme colors in color mode, grayscale in B&W mode
  - Ensure PDF export respects current filter state
  - _Requirements: 3.1, 3.5, 3.6, 3.7, 3.8, 8.1, 8.2_

- [ ] 12. Implement DIU Excel parser for complex routine files
  - Create DIUExcelParser class to handle multi-sheet Excel parsing
  - Implement logic to extract weekday from sheet names
  - Parse theory class slots from Row 2 headers (every 3 columns)
  - Extract theory class data from rows 4-16 with room numbers from Column B
  - Parse lab class slots from Row 18 headers (every 3 columns)
  - Extract lab class data from rows 20-31 and 34-35 with room numbers from Column B
  - Implement 3-column group parsing (Level-Term, Course-Section, Teacher)
  - Extract section from last character of course-section cell
  - Remove section character from course code before storing
  - Extract section from last character of Level-Term and remove it
  - Skip empty cells and continue processing valid data
  - Combine data from all sheets into unified dataset
  - Add comprehensive error handling for malformed Excel files
  - _Requirements: 4.1, 4.2, 4.3, 4.4, 4.5, 4.6, 4.7, 4.8, 4.9_


- [ ] 13. Add DIU branding to application header
  - Embed DIU logo as base64 data URI or use text-based branding
  - Update header to display "DIU EEE Routine Manager" with gradient effect
  - Style header with EEE theme colors and modern design
  - Ensure header is responsive and looks good on all devices
  - _Requirements: 8.3_



- [ ] 14. Create PDF export options modal
  - Design modal for PDF export settings with color vs B&W radio buttons
  - Integrate column selection into PDF export modal
  - Add preview text showing selected options
  - Style modal with EEE theme
  - Wire up modal to PDF export function with selected options
  - _Requirements: 3.2, 3.6_

- [ ] 15. Update export buttons and integrate with modals
  - Remove CSV export button from header controls
  - Keep Excel export and PDF export buttons
  - Wire up Excel export button to show column selection modal
  - Wire up PDF export button to show PDF options modal (color/B&W + columns)
  - Ensure all export buttons have consistent EEE-themed styling
  - _Requirements: 3.1, 3.2, 6.3_

- [ ] 16. Add comprehensive error handling and user feedback
  - Implement user-friendly error messages for all import/export operations
  - Add loading states and progress indicators for long-running operations
  - Create success notifications and confirmation messages for user actions
  - Add specific error messages for DIU Excel parsing failures
  - Ensure consistent error handling patterns throughout the application
  - _Requirements: 3.9, 4.9, 5.5_

- [ ] 17. Final integration testing and performance optimization
  - Test DIU Excel import with actual Routine_version_4 update.xls file
  - Verify multi-sheet parsing extracts all theory and lab classes correctly
  - Test PDF export in both color and B&W modes with DIU branding
  - Verify localStorage migration works correctly with existing user data
  - Test all export formats (Excel, PDF) with column selection
  - Verify combined time and level-term fields work throughout the app
  - Optimize table rendering and filtering performance for large datasets
  - Test responsive design across different screen sizes and devices
  - Verify EEE theme colors and animations work smoothly
  - Test single-file HTML portability
  - _Requirements: 9.1, 9.2, 9.3, 9.4, 9.5_