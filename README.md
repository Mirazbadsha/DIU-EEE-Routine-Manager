# 🎓 DIU EEE Routine Manager

<div align="center">

![DIU Logo](https://img.shields.io/badge/DIU-EEE%20Department-00d4ff?style=for-the-badge)
![Version](https://img.shields.io/badge/version-7.0-34d399?style=for-the-badge)
![License](https://img.shields.io/badge/license-MIT-fbbf24?style=for-the-badge)

**A professional, feature-rich routine management system for Daffodil International University's Electrical & Electronic Engineering Department**

[Features](#-features) • [Demo](#-demo) • [Installation](#-installation) • [Usage](#-usage) • [Admin Guide](#-admin-guide)

</div>

---

## 📋 Overview

DIU EEE Routine Manager is a modern, browser-based application designed to streamline class schedule management for students and faculty. Built with vanilla JavaScript and featuring a beautiful dark-themed UI, it offers powerful import/export capabilities, advanced filtering, and secure admin controls.

### ✨ Key Highlights

- 🎨 **Modern Dark UI** - Beautiful gradient design with smooth animations
- 📊 **Excel Import/Export** - Direct import from DIU routine Excel files
- 🔍 **Advanced Filtering** - Filter by day, time, room, teacher, section, and more
- 👨‍🏫 **Teacher Information** - Clickable teacher names with contact details
- 📚 **Course Titles** - Auto-fill course names from imported data
- 🏫 **Room Finder** - Find available rooms by day and time slot
- 🔐 **Admin Panel** - Secure access control for data management
- 📱 **Fully Responsive** - Works seamlessly on desktop, tablet, and mobile
- 💾 **Auto-Save** - Data automatically saved to browser storage
- 📄 **PDF Export** - Generate professional PDF reports with color/B&W options

---

## 🚀 Features

### For Students & Faculty

#### 📅 Routine Management
- View complete class schedules in an organized table
- Sort by any column (day, time, room, teacher, etc.)
- Filter classes by multiple criteria simultaneously
- Search for specific courses, teachers, or sections

#### 🏫 Room Availability
- Real-time room availability checker
- Find empty rooms for specific days and time slots
- Visual badges for available rooms

#### 👨‍🏫 Teacher Information
- Click on any teacher's initials to view details
- Access phone numbers and email addresses
- Direct links to call or email teachers

#### 📊 Export Options
- **Excel Export**: Customizable column selection
- **PDF Export**: Color or black & white options
- **JSON Backup**: Complete data backup for restoration

### For Administrators

#### 🔐 Secure Admin Panel
- Email and password authentication
- Session-based access control (30-minute timeout)
- Automatic logout on browser close

#### 📥 Data Import
- Import routine from DIU Excel files
- Import teacher contact information
- Import course titles for auto-completion
- Restore from JSON backups

#### ✏️ Data Management
- Add new classes manually
- Edit existing class entries
- Delete individual classes
- Clear all data with confirmation

#### 🛡️ Access Control
- Admin-only features hidden from regular users
- Public features remain accessible to everyone
- Session persistence across page refreshes

---

## 🎯 Demo

### Screenshots

**Main Interface**
```
┌─────────────────────────────────────────────────────────┐
│  🎓 DIU EEE Routine Manager                             │
│  [Developer] [Export Excel] [Export PDF] [Admin Login]  │
└─────────────────────────────────────────────────────────┘

┌─────────────────────────────────────────────────────────┐
│  Filters: Day | Type | Time | Room | Level-Term | ...   │
│  🏫 Available Rooms: [301] [302] [305] [401]            │
└─────────────────────────────────────────────────────────┘

┌─────────────────────────────────────────────────────────┐
│  Day    │ Type   │ Time      │ Room │ Teacher │ ...     │
│─────────┼────────┼───────────┼──────┼─────────┼─────────│
│ Saturday│ Theory │ 8:20-9:50 │ 301  │ MRK     │ ...     │
│ Sunday  │ Lab    │ 10:00-... │ 401  │ SRC     │ ...     │
└─────────────────────────────────────────────────────────┘
```

---

## 📦 Installation

### Quick Start

1. **Download the project**
   ```bash
   git clone https://github.com/yourusername/diu-eee-routine-manager.git
   cd diu-eee-routine-manager
   ```

2. **Add required files**
   - Place `diu-logo.png` in the project root
   - Place `devloper.jpg` (developer photo) in the project root

3. **Open in browser**
   ```bash
   # Simply open v7.html in any modern browser
   # No build process or server required!
   ```

### Requirements

- Modern web browser (Chrome, Firefox, Safari, Edge)
- JavaScript enabled
- No server or backend required
- No npm packages or dependencies

---

## 📖 Usage

### For Regular Users

#### Viewing the Routine

1. Open `v7.html` in your browser
2. The routine table displays all classes
3. Click column headers to sort
4. Use filters to narrow down results

#### Filtering Classes

```
1. Select filters from the dropdown menus
2. Type in text fields for partial matches
3. Multiple filters work together (AND logic)
4. Click "Clear All" to reset filters
```

#### Finding Empty Rooms

```
1. Select a Day from the filter
2. Select a Time Slot from the filter
3. Available rooms appear in the "Available Rooms" section
```

#### Viewing Teacher Details

```
1. Click on any teacher's initials (e.g., "MRK")
2. Modal opens with full name, phone, and email
3. Click phone/email to call or send email directly
```

#### Exporting Data

**Excel Export:**
1. Click "📊 Export Excel"
2. Select columns to include
3. Click "Continue"
4. File downloads automatically

**PDF Export:**
1. Click "📄 Export PDF"
2. Select columns to include
3. Choose color mode (Color/B&W)
4. Click "Export PDF"

---

## 🔐 Admin Guide

### Logging In

1. Click "🔐 Admin Login" in the header
2. Enter credentials:
   - **Email**: `admin@diu.edu.bd`
   - **Password**: `admin123`
3. Click "Login"

> ⚠️ **Security Note**: Change default credentials in production!

### Importing Data

#### Import Routine (Excel)
```
1. Click "📊 Import Routine"
2. Select DIU routine Excel file
3. Data automatically parsed and imported
4. Classes appear in the table
```

**Expected Excel Format:**
- Sheets named by day (Saturday, Sunday, etc.)
- Theory classes in rows 4-16
- Lab classes in rows 20-31, 34-35
- Columns: Room, Level-Term, Course-Section, Teacher

#### Import Teacher Info
```
1. Click "👨‍🏫 Import Teacher Info"
2. Select Excel file with columns:
   - Initial (e.g., "MRK")
   - Name
   - Contact No.
   - Email
3. Teacher names become clickable in the table
```

#### Import Course Titles
```
1. Click "📚 Import Course Titles"
2. Select Excel file with columns:
   - Course Code (e.g., "EEE-101")
   - Course Title
3. Course titles auto-fill when adding/importing classes
```

### Managing Classes

#### Add New Class
```
1. Click "➕ Add Class"
2. Fill in the form:
   - Day, Type, Time Slot
   - Room, Level-Term, Section
   - Class Code, Course Title
   - Teacher, Notes (optional)
3. Click "Save"
```

#### Edit Class
```
1. Click "Edit" button on any row
2. Modify fields as needed
3. Click "Save"
```

#### Delete Class
```
1. Click "Delete" button on any row
2. Confirm deletion
```

### Backup & Restore

#### Create Backup
```
1. Click "💾 Backup JSON"
2. JSON file downloads with timestamp
3. Store safely for future restoration
```

#### Restore from Backup
```
1. Click "📥 Restore JSON"
2. Select previously saved JSON file
3. Data replaces current routine
```

### Logging Out

```
1. Click "🚪 Logout" button
2. Confirm logout
3. Admin features become hidden
4. Session cleared
```

---

## 🛠️ Technical Details

### Technology Stack

- **Frontend**: Vanilla JavaScript (ES6+)
- **Styling**: CSS3 with CSS Variables
- **Storage**: Browser LocalStorage & SessionStorage
- **Libraries**:
  - [SheetJS (xlsx)](https://sheetjs.com/) - Excel file processing
  - [jsPDF](https://github.com/parallax/jsPDF) - PDF generation
  - [jsPDF-AutoTable](https://github.com/simonbengtsson/jsPDF-AutoTable) - PDF tables

### Data Storage

```javascript
// LocalStorage Keys
'diu_eee_routine_v3'        // Main routine data
'diu_eee_teacher_data'      // Teacher information
'diu_eee_course_titles'     // Course title mappings

// SessionStorage Keys
'diu_eee_admin_session'     // Admin authentication
```

### Browser Compatibility

| Browser | Version | Status |
|---------|---------|--------|
| Chrome  | 90+     | ✅ Full Support |
| Firefox | 88+     | ✅ Full Support |
| Safari  | 14+     | ✅ Full Support |
| Edge    | 90+     | ✅ Full Support |

---

## 🎨 Customization

### Changing Admin Credentials

Edit `v7.html` and find:

```javascript
const ADMIN_CREDENTIALS = {
    email: 'admin@diu.edu.bd',     // Change this
    password: 'admin123'            // Change this
};
```

### Customizing Colors

Edit CSS variables in `v7.html`:

```css
:root {
    --accent-cyan: #22d3ee;        /* Primary accent */
    --accent-green: #34d399;       /* Secondary accent */
    --bg-primary: #0a1628;         /* Background */
    /* ... more variables ... */
}
```

### Adding Custom Columns

1. Edit the `COLUMNS` array in JavaScript
2. Add your column definition:
```javascript
{ 
    key: 'myColumn', 
    label: 'My Column', 
    type: 'text', 
    placeholder: 'Enter value' 
}
```

---

## 🤝 Contributing

Contributions are welcome! Here's how you can help:

1. **Report Bugs**: Open an issue with details
2. **Suggest Features**: Share your ideas
3. **Submit PRs**: Fork, create branch, commit, push, PR
4. **Improve Docs**: Help make documentation better

### Development Setup

```bash
# Clone the repository
git clone https://github.com/yourusername/diu-eee-routine-manager.git

# No build process needed - just edit v7.html
# Open in browser to test changes
```

---

## 📝 License

This project is licensed under the MIT License - see below for details:

```
MIT License

Copyright (c) 2025 Sheikh Miraz Uddin Badsha

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all
copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE
SOFTWARE.
```

---

## 👨‍💻 Developer

**Sheikh Miraz Uddin Badsha**
- 🎓 Batch: 221(37), EEE Department
- 🏫 Daffodil International University
- 💼 [LinkedIn](https://www.linkedin.com/in/sheikh-miraz-uddin-badsha-b813621b4)
- 📧 [badsha33-1686@diu.edu.bd](mailto:badsha33-1686@diu.edu.bd)

---

## 🙏 Acknowledgments

- Daffodil International University
- EEE Department Faculty
- All students who provided feedback
- Open source library contributors

---

## 📞 Support

Need help? Have questions?

- 📧 Email: badsha33-1686@diu.edu.bd
- 💼 LinkedIn: [Sheikh Miraz Uddin Badsha](https://www.linkedin.com/in/sheikh-miraz-uddin-badsha-b813621b4)
- 🐛 Issues: [GitHub Issues](https://github.com/yourusername/diu-eee-routine-manager/issues)

---

## 🗺️ Roadmap

### Upcoming Features

- [ ] Multi-user admin support
- [ ] Password encryption
- [ ] Export to Google Calendar
- [ ] Mobile app version
- [ ] Dark/Light theme toggle
- [ ] Notification system for class changes
- [ ] Integration with DIU student portal

---

<div align="center">

**Made with ❤️ for DIU EEE Department**

⭐ Star this repo if you find it helpful!

[Report Bug](https://github.com/yourusername/diu-eee-routine-manager/issues) • [Request Feature](https://github.com/yourusername/diu-eee-routine-manager/issues)

</div>
