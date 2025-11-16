# Contributing to DIU EEE Routine Manager

First off, thank you for considering contributing to DIU EEE Routine Manager! 🎉

## 📋 Table of Contents

- [Code of Conduct](#code-of-conduct)
- [How Can I Contribute?](#how-can-i-contribute)
- [Development Setup](#development-setup)
- [Coding Guidelines](#coding-guidelines)
- [Commit Guidelines](#commit-guidelines)
- [Pull Request Process](#pull-request-process)

## 📜 Code of Conduct

This project and everyone participating in it is governed by respect and professionalism. By participating, you are expected to uphold this standard.

## 🤝 How Can I Contribute?

### Reporting Bugs

Before creating bug reports, please check existing issues. When creating a bug report, include:

- **Clear title and description**
- **Steps to reproduce** the issue
- **Expected behavior** vs **actual behavior**
- **Screenshots** if applicable
- **Browser and version** information

**Example:**
```markdown
**Bug**: Filter not working for teacher names

**Steps to Reproduce:**
1. Open v7.html
2. Select teacher "MRK" from filter
3. Click apply

**Expected:** Show only MRK's classes
**Actual:** Shows all classes

**Browser:** Chrome 120.0
```

### Suggesting Features

Feature suggestions are welcome! Please include:

- **Clear use case** - Why is this needed?
- **Proposed solution** - How should it work?
- **Alternatives considered** - What other options did you think about?
- **Additional context** - Screenshots, mockups, etc.

### Code Contributions

1. **Fork the repository**
2. **Create a feature branch** (`git checkout -b feature/AmazingFeature`)
3. **Make your changes**
4. **Test thoroughly**
5. **Commit your changes** (`git commit -m 'Add some AmazingFeature'`)
6. **Push to the branch** (`git push origin feature/AmazingFeature`)
7. **Open a Pull Request**

## 🛠️ Development Setup

### Prerequisites

- Modern web browser (Chrome, Firefox, Safari, Edge)
- Text editor or IDE (VS Code, Sublime, etc.)
- Git for version control

### Getting Started

```bash
# Clone your fork
git clone https://github.com/YOUR_USERNAME/diu-eee-routine-manager.git

# Navigate to directory
cd diu-eee-routine-manager

# Create a new branch
git checkout -b feature/my-new-feature

# Make changes to v7.html

# Test in browser
# Simply open v7.html in your browser

# Commit and push
git add .
git commit -m "Add my new feature"
git push origin feature/my-new-feature
```

### Project Structure

```
diu-eee-routine-manager/
├── v7.html                          # Main application file
├── diu-logo.png                     # University logo
├── devloper.jpg                     # Developer photo
├── README.md                        # Project documentation
├── LICENSE                          # MIT License
├── CONTRIBUTING.md                  # This file
├── .gitignore                       # Git ignore rules
└── .kiro/                          # IDE configuration (ignored)
```

## 📝 Coding Guidelines

### JavaScript Style

```javascript
// Use const for constants
const STORAGE_KEY = 'diu_eee_routine_v3';

// Use let for variables
let rows = [];

// Use arrow functions
const filterData = (data) => {
    return data.filter(item => item.active);
};

// Use template literals
const message = `Loaded ${count} classes`;

// Use meaningful variable names
const isAdminLoggedIn = () => { /* ... */ };

// Add comments for complex logic
// Check if session is still valid (within 30 minutes)
const sessionAge = now - data.timestamp;
```

### CSS Style

```css
/* Use CSS variables for colors */
:root {
    --accent-cyan: #22d3ee;
}

/* Use BEM-like naming for classes */
.modal-overlay { }
.modal-header { }
.modal-body { }

/* Group related properties */
.button {
    /* Display & Box Model */
    display: inline-flex;
    padding: 10px 18px;
    
    /* Visual */
    background: var(--surface-secondary);
    border: 1px solid var(--border-primary);
    
    /* Typography */
    font-size: 13px;
    font-weight: 500;
    
    /* Animation */
    transition: all 0.3s ease;
}
```

### HTML Style

```html
<!-- Use semantic HTML -->
<header class="header">
    <nav class="header-actions">
        <button class="btn btn-sm" id="btnExportExcel">
            📊 Export Excel
        </button>
    </nav>
</header>

<!-- Use meaningful IDs and classes -->
<div class="modal-overlay hidden" id="adminLoginModal">
    <!-- Modal content -->
</div>

<!-- Add accessibility attributes -->
<button aria-label="Close modal" class="close-btn">×</button>
```

## 💬 Commit Guidelines

### Commit Message Format

```
<type>(<scope>): <subject>

<body>

<footer>
```

### Types

- **feat**: New feature
- **fix**: Bug fix
- **docs**: Documentation changes
- **style**: Code style changes (formatting, etc.)
- **refactor**: Code refactoring
- **test**: Adding or updating tests
- **chore**: Maintenance tasks

### Examples

```bash
# Good commit messages
git commit -m "feat(filters): add teacher name autocomplete"
git commit -m "fix(export): resolve PDF generation error"
git commit -m "docs(readme): update installation instructions"
git commit -m "style(css): improve button hover effects"

# Bad commit messages (avoid these)
git commit -m "fixed stuff"
git commit -m "updates"
git commit -m "asdfgh"
```

## 🔄 Pull Request Process

### Before Submitting

- [ ] Test your changes in multiple browsers
- [ ] Ensure no console errors
- [ ] Update documentation if needed
- [ ] Follow coding guidelines
- [ ] Write clear commit messages

### PR Template

```markdown
## Description
Brief description of changes

## Type of Change
- [ ] Bug fix
- [ ] New feature
- [ ] Documentation update
- [ ] Code refactoring

## Testing
- [ ] Tested in Chrome
- [ ] Tested in Firefox
- [ ] Tested on mobile
- [ ] No console errors

## Screenshots (if applicable)
Add screenshots here

## Checklist
- [ ] Code follows style guidelines
- [ ] Self-review completed
- [ ] Documentation updated
- [ ] No breaking changes
```

### Review Process

1. Maintainer will review your PR
2. Address any requested changes
3. Once approved, PR will be merged
4. Your contribution will be credited!

## 🎯 Areas for Contribution

### High Priority

- [ ] Add unit tests
- [ ] Improve mobile responsiveness
- [ ] Add keyboard shortcuts
- [ ] Implement undo/redo functionality
- [ ] Add data validation

### Medium Priority

- [ ] Dark/Light theme toggle
- [ ] Export to Google Calendar
- [ ] Bulk edit functionality
- [ ] Advanced search features
- [ ] Print-friendly view

### Low Priority

- [ ] Animations and transitions
- [ ] Custom color themes
- [ ] Internationalization (i18n)
- [ ] Accessibility improvements
- [ ] Performance optimizations

## 📞 Questions?

If you have questions, feel free to:

- Open an issue with the "question" label
- Email: badsha33-1686@diu.edu.bd
- LinkedIn: [Sheikh Miraz Uddin Badsha](https://www.linkedin.com/in/sheikh-miraz-uddin-badsha-b813621b4)

## 🙏 Thank You!

Your contributions make this project better for everyone. Thank you for taking the time to contribute! 🎉

---

**Happy Coding!** 💻✨
