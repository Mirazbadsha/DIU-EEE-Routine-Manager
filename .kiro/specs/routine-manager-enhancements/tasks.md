# Implementation Plan

- [ ] 1. Add Course Title and Class Code to filter system
  - Update COLUMNS array to ensure classCode and courseTitle are included with proper configuration
  - Modify renderFilters() function to generate filter fields for these columns
  - Update getUniqueValues() to extract unique course codes and titles from data
  - Test that new filter fields appear in the filter panel
  - _Requirements: 1.1, 1.3_

- [ ] 2. Implement hybrid filter fields with keyword search capability
  - Replace select dropdowns with input fields that support datalist for autocomplete
  - Update filter field HTML generation to use input + datalist pattern
  - Modify filter event listeners to handle text input changes
  - Implement real-time filtering as user types in filter fields
  - Test keyword search with partial matches across all filter fields
  - _Requirements: 1.2, 1.4, 1.5_

- [ ] 3. Create Empty Room Finder module
  - Implement RoomFinder object with getAllRooms() method
  - Implement findEmptyRooms(day, timeSlot) method to analyze schedule data
  - Implement getCurrentFilters() method to read active filter state
  - Write unit tests for room availability calculation logic
  - _Requirements: 2.2, 2.3, 2.4_

- [ ] 4. Build Empty Room Finder UI component
  - Add HTML structure for empty room finder section in filter panel
  - Create CSS styles for room badges and empty room display
  - Implement updateEmptyRooms() function to render available rooms
  - Add logic to show appropriate messages when no filters selected or no rooms available
  - Integrate updateEmptyRooms() call into filter change event handlers
  - Test empty room display with various filter combinations
  - _Requirements: 2.1, 2.5, 2.6, 2.7_

- [ ] 5. Implement teacher info Excel parser
  - Create parseTeacherInfoExcel() function to extract teacher data from Excel
  - Map teacher data structure with initial, name, phone, and email fields
  - Add file input for teacher info Excel import in control panel
  - Implement handleTeacherImport() function to process uploaded file
  - Store parsed teacher data in global teacherData object
  - Add error handling for missing columns or invalid data
  - Test with provided teacher info.xls file
  - _Requirements: 3.1, 3.2, 3.9_

- [ ] 6. Create clickable teacher initial links in table
  - Modify renderTable() to use renderTeacherCell() for teacher column
  - Implement renderTeacherCell() to generate clickable links for teachers with data
  - Add CSS styles for teacher-link class with hover effects
  - Implement event delegation for teacher link clicks
  - Test that only teachers with loaded data show as clickable
  - _Requirements: 3.3, 3.8_

- [ ] 7. Build teacher details modal component
  - Add HTML structure for teacher modal with detail rows
  - Create CSS styles for teacher modal and detail layout
  - Implement showTeacherModal(initial) function to populate and display modal
  - Add close button functionality for teacher modal
  - Implement modal overlay click to close
  - Test modal display with complete and incomplete teacher data
  - _Requirements: 3.4, 3.5_

- [ ] 8. Implement contact action links in teacher modal
  - Create mailto: links for email addresses in modal
  - Create tel: links for phone numbers in modal
  - Add click handlers to open email client when email is clicked
  - Add click handlers to initiate phone call when phone is clicked
  - Style contact links with appropriate visual indicators
  - Test email and phone link functionality on desktop and mobile
  - _Requirements: 3.6, 3.7_

- [ ] 9. Update CSS color variables for improved contrast
  - Modify --text-primary to pure white (#ffffff) for better readability
  - Update --text-secondary to lighter shade (#e2e8f0)
  - Adjust --text-muted to improved contrast (#a0aec0)
  - Brighten --accent-cyan and --accent-green for better visibility
  - Increase --border-primary visibility (#475569)
  - Test contrast ratios meet WCAG AA standards (4.5:1 minimum)
  - _Requirements: 4.2, 4.4_

- [ ] 10. Enhance typography and text styling
  - Add -webkit-font-smoothing and -moz-osx-font-smoothing to body
  - Update tbody td color to use --text-primary
  - Increase font-weight for filter and form labels to 600
  - Adjust header gradient with brighter colors and filter brightness
  - Update button text colors for better visibility
  - Test text readability across all components
  - _Requirements: 4.1, 4.3, 4.5, 4.6, 4.7_

- [ ] 11. Integrate all features and perform end-to-end testing
  - Test complete workflow: import routine → import teacher info → apply filters → check empty rooms → click teacher
  - Verify localStorage persistence works with new features
  - Test responsive behavior on mobile devices
  - Verify all modals work correctly (row editor, column selection, PDF options, teacher details)
  - Test error handling for missing data scenarios
  - Verify export functionality (Excel, PDF, JSON) still works correctly
  - _Requirements: All_
