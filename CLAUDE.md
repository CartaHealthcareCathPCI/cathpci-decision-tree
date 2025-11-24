# CathPCI Decision Tree - Claude Integration Guide

*Last Updated: 2025-11-24*
*Version: 2.2*

## Project Overview

The CathPCI Decision Tree is an interactive web-based application designed for healthcare professionals working with the National Cardiovascular Data Registry (NCDR) Catheterization Percutaneous Coronary Intervention program. This tool provides a guided decision-making process to determine appropriate **Cath Lab Indications** and **PCI Indications** for cardiac catheterization procedures.

### Purpose

This application helps clinicians:
1. Navigate through a 7-step decision tree to identify appropriate Cath Lab Indications based on patient presentation
2. Automatically determine the corresponding PCI Indication using priority-based clinical logic
3. Ensure compliance with NCDR CathPCI registry requirements through built-in validation

### Live Deployment

- **Production URL**: `https://CartaHealthcareCathPCI.github.io/cathpci-decision-tree/`
- **Deployment**: Automatic via GitHub Actions on push to `main` branch

---

## Repository Structure

```
cathpci-decision-tree/
├── .github/
│   └── workflows/
│       └── deploy-pages.yml      # GitHub Actions workflow for deployment
├── index.html                    # Main application (1202 lines)
├── README.md                     # Project overview and usage documentation
├── claude.md                     # Legacy AI integration guide (deprecated)
└── CLAUDE.md                     # This file - comprehensive AI assistant guide
```

### Key Files

- **index.html** (1202 lines): Complete self-contained application
  - Lines 10-372: CSS styling with gradient backgrounds, responsive design, and orange section headings
  - Lines 374-691: HTML structure for 7-step decision tree interface with section headings
  - Lines 692-1202: JavaScript logic for state management, validation, and PCI determination

---

## Technical Architecture

### Technology Stack

- **Frontend**: Pure HTML5, CSS3, Vanilla JavaScript (ES6+)
- **Fonts**: Space Grotesk from Google Fonts
- **Styling**: CSS Grid, Flexbox, Gradient backgrounds, Orange accent color (#FF6720)
- **Visual Design**: Orange section headings for each decision tree step, left border accents
- **State Management**: JavaScript Set for selections, Object for sub-answers
- **Deployment**: GitHub Pages with GitHub Actions CI/CD
- **Version Control**: Git with feature branch workflow

### Why No Frameworks?

This is intentional design:
- **Zero dependencies**: No npm, no build process, no external libraries
- **Instant deployment**: Edit index.html and refresh browser
- **Maximum portability**: Works anywhere HTML works (hospital intranets, offline, etc.)
- **Security**: No supply chain vulnerabilities from dependencies
- **Simplicity**: Easy to audit, modify, and maintain

---

## Clinical Decision Logic

### 7-Step Decision Process

Each step is visually organized with an orange section heading for improved navigation:

1. **Step 1: Acute Coronary Syndrome** - ACS evaluation with timing (<24hrs vs >24hrs)
   - Lists STEMI, NSTEMI, and Unstable Angina on separate lines
2. **Step 2: Angina** - Recent or ongoing chest pain or explicitly documented anginal equivalents
   - New onset vs worsening angina with history assessment
3. **Step 3: Coronary Artery Disease History** - Stable known CAD vs suspected CAD evaluation
4. **Step 4: Cardiac Arrest** - Resuscitated cardiac arrest screening
5. **Step 5: Non-Ischemic/Structural Heart Disease** - Multiple selections allowed (valvular, pericardial, arrhythmia, cardiomyopathy, LV dysfunction, syncope, post-transplant)
6. **Step 6: Non-Cardiac/Evaluation** - Pre-operative evaluation or exercise clearance
7. **Step 7: Other Indications** - Catch-all for indications not covered above

### Indication Priority Hierarchy

When multiple indications are selected, the PCI indication is determined by the **highest priority** Cath Lab Indication:

1. ACS ≤ 24 hrs (highest priority)
2. ACS > 24 hrs
3. New Onset Angina ≤ 2 months
4. Stable Known CAD
5. Suspected CAD
6. Worsening Angina
7. Resuscitated Cardiac Arrest
8. Valvular Disease
9. Pericardial Disease
10. Cardiac arrhythmia
11. Cardiomyopathy
12. LV Dysfunction
13. Syncope
14. Post Cardiac Transplant
15. Pre-operative Evaluation
16. Evaluation for Exercise Clearance
17. Other (lowest priority)

### PCI Indication Mapping Rules

**Complex Mappings (with sub-questions):**

- **ACS ≤ 24 hrs** → Requires STEMI sub-question:
  - If STEMI: Timing options (Immediate, Stable ≤12hrs, Stable >12hrs, Unstable, After lytics, Rescue)
  - If No: NSTE-ACS

- **ACS > 24 hrs** → Requires STEMI sub-question:
  - If STEMI: Timing options (Stable >12hrs, Unstable, After lytics, Rescue)
  - If No: NSTE-ACS

- **Stable Known CAD** → Requires angina sub-question:
  - If Yes: Stable angina
  - If No: CAD (without ischemic Sx)

**Direct Mappings:**

- New Onset Angina ≤ 2 months → Direct PCI mapping
- All others → Other PCI Indication

### Validation Rules

The application enforces clinical validation rules to prevent incompatible selections:

```javascript
// Example incompatibility: ACS ≤24hrs cannot coexist with ACS >24hrs
const validationRules = {
  'ACS ≤ 24 hrs': ['ACS > 24 hrs'],
  'ACS > 24 hrs': ['ACS ≤ 24 hrs'],
  'Suspected CAD': ['Stable Known CAD'],
  'Stable Known CAD': ['Suspected CAD']
};
```

---

## Key JavaScript Functions

### State Management

- `selectIndication(indication, elementId)` - Adds indication to selectedIndications Set
- `removeIndication(indication)` - Removes indication and updates UI
- `updateSummary()` - Rebuilds the summary display of all selected indications
- `resetForm()` - Clears all selections and returns to initial state

### Validation & Logic

- `validateSelections()` - Checks for incompatible indication combinations
- `displayErrors(errors)` - Shows validation errors to user
- `determinePCIIndication()` - Calculates appropriate PCI indication based on priority
- `showPCISubQuestion(questionType)` - Displays conditional sub-questions
- `handlePCISubAnswer(questionType, answer)` - Processes sub-question responses

### UI State

- `updateArrows()` - Updates completion arrows for all steps
- `updateArrowState(stepNum, isComplete)` - Sets individual step completion state
- `toggleRadioGroup(groupName, value)` - Handles radio button selections
- `toggleCheckbox(checkboxId)` - Handles checkbox selections

### Reminders

- `showReminder(indication)` - Displays contextual help for specific selections
- `hideReminder(indication)` - Hides contextual help

---

## Development Workflow

### Git Branching Strategy

This repository uses a **feature branch workflow**:

1. **Main Branch**: `main` - production-ready code, auto-deploys to GitHub Pages
2. **Feature Branches**: `claude/{description}-{session-id}` - for development work
3. **Pull Requests**: Required for merging to main

### Branch Naming Convention

All Claude-created branches follow this pattern:
```
claude/{feature-description}-{unique-session-id}
```

Examples from git history:
- `claude/update-sidebar-color-017GEegBuiT5NKY97pmii5hQ`
- `claude/setup-hosted-deployment-01V8v1AcxLkJfvLXHwAoWkxz`
- `claude/write-claude-integration-01R8XzcNWpaMqjkcJCta94pZ`

### Development Process

1. **Create Feature Branch**:
   ```bash
   git checkout -b claude/your-feature-name-{session-id}
   ```

2. **Make Changes**: Edit `index.html` directly

3. **Test Locally**: Open `index.html` in browser

4. **Commit Changes**:
   ```bash
   git add index.html
   git commit -m "Descriptive commit message"
   ```

5. **Push to Remote**:
   ```bash
   git push -u origin claude/your-feature-name-{session-id}
   ```

6. **Create Pull Request**: Merge to `main` via GitHub PR

7. **Auto-Deploy**: GitHub Actions automatically deploys to GitHub Pages

### CI/CD Pipeline

**Workflow File**: `.github/workflows/deploy-pages.yml`

**Trigger Events**:
- Push to `main` branch
- Manual workflow dispatch

**Deployment Steps**:
1. Checkout repository
2. Setup GitHub Pages
3. Upload artifact (entire repository)
4. Deploy to GitHub Pages

**Permissions Required**:
- `contents: read`
- `pages: write`
- `id-token: write`

---

## AI Assistant Guidelines

### When Making Changes

#### ✅ DO:

1. **Read Before Modifying**: Always read `index.html` before making any changes
2. **Preserve Clinical Logic**: All validation rules are based on NCDR CathPCI registry requirements - don't modify without clinical justification
3. **Test Validation**: After changes, mentally walk through test cases to ensure validation still works
4. **Maintain State Management**: Respect the `selectedIndications` Set and `pciSubAnswers` object
5. **Keep It Simple**: No frameworks, no build process, no dependencies
6. **Responsive Design**: Ensure changes work on desktop, tablet, and mobile
7. **Consistent Styling**: Follow existing Space Grotesk font, gradient color scheme, and orange accent color (#FF6720) for section headings and highlights
8. **Comment Complex Logic**: Add comments for any non-obvious clinical decision logic
9. **Update This Doc**: If you make significant architectural changes, update CLAUDE.md

#### ❌ DON'T:

1. **Don't Add Frameworks**: No React, Vue, Angular, jQuery, etc.
2. **Don't Add Build Tools**: No npm, webpack, vite, etc.
3. **Don't Break Validation**: Don't remove validation rules without understanding clinical implications
4. **Don't Change Priority Logic**: The indication priority order is clinically defined
5. **Don't Create Separate Files**: Keep everything in `index.html` (unless there's a very good reason)
6. **Don't Remove Comments**: Preserve existing code comments that explain clinical logic
7. **Don't Push to Main**: Always use feature branches and pull requests
8. **Don't Ignore Responsive Design**: Test that UI works on small screens

### Code Style Conventions

**JavaScript:**
```javascript
// Use descriptive variable names
const selectedIndications = new Set();

// Use template literals for dynamic content
element.innerHTML = `<div>${indication}</div>`;

// Use const/let, not var
const priority = 1;
let currentStep = 0;

// Use arrow functions for callbacks
options.forEach(option => {
    handleOption(option);
});

// Add comments for complex clinical logic
// ACS ≤24hrs has highest priority per NCDR guidelines
if (selectedIndications.has('ACS ≤ 24 hrs')) {
    return determinePCIForACS();
}
```

**CSS:**
```css
/* Use the established orange accent color for highlights and section headings */
.section-heading {
    background: #FF6720;  /* Orange brand color */
    color: white;
    padding: 10px 20px;
}

/* Maintain consistent border styling with orange accents */
.step-section {
    border-left: 4px solid #FF6720;
}
```

### Testing Scenarios

After making changes, manually test these scenarios:

#### Test Case 1: STEMI Patient (Priority Path)
```
Step 1: Select "ACS ≤ 24 hrs" → Yes
Sub-question: STEMI → Yes
Sub-question: Timing → Immediate PCI
Expected PCI: "STEMI - Immediate PCI for Acute STEMI"
```

#### Test Case 2: Stable CAD without Symptoms
```
Step 3: CAD History → Yes → Stable Known CAD
Sub-question: Angina → No
Expected PCI: "CAD (without ischemic Sx)"
```

#### Test Case 3: Valvular Disease with Reminder
```
Step 5: Select "Valvular Disease"
Expected: Reminder about stenosis/regurgitation details appears
Expected PCI: "Other PCI Indication"
```

#### Test Case 4: Multiple Indications (Priority Test)
```
Step 5: Select "Valvular Disease"
Step 5: Select "LV Dysfunction"
Step 3: Select "Suspected CAD"
Expected: All three appear in summary
Expected PCI: Based on "Suspected CAD" (higher priority than structural issues)
```

#### Test Case 5: Validation Error
```
Step 1: Select "ACS ≤ 24 hrs" → Yes
Step 1: Try to also select "ACS > 24 hrs" → Yes
Expected: Validation error prevents incompatible selection
```

#### Test Case 6: Reset Functionality
```
Select multiple indications
Click "Reset"
Expected: All selections cleared, form returns to initial state
```

---

## Common Issues and Solutions

### Issue: Changes Not Appearing After Edit

**Cause**: Browser caching
**Solution**: Hard refresh (Ctrl+Shift+R / Cmd+Shift+R) or open in incognito mode

### Issue: Validation Not Working

**Cause**: JavaScript error in console
**Solution**:
1. Open browser DevTools (F12)
2. Check Console tab for errors
3. Fix JavaScript syntax errors
4. Ensure all functions are properly defined

### Issue: PCI Indication Not Updating

**Cause**: `determinePCIIndication()` not being called after selection change
**Solution**:
1. Ensure `selectIndication()` calls `determinePCIIndication()`
2. Check that priority logic is intact
3. Verify sub-question handlers are working

### Issue: Styling Broken on Mobile

**Cause**: CSS media queries not working or viewport meta tag missing
**Solution**:
1. Verify `<meta name="viewport">` tag exists in `<head>`
2. Check responsive CSS rules
3. Test on actual mobile device, not just browser DevTools

### Issue: GitHub Pages Not Updating

**Cause**: Deployment workflow not triggered or failed
**Solution**:
1. Check Actions tab in GitHub repository
2. Verify workflow file syntax in `.github/workflows/deploy-pages.yml`
3. Ensure changes were pushed to `main` branch
4. Wait 2-3 minutes for GitHub Pages to update

---

## Recent Changes History

### 2025-11-24 (Current Version)
- Updated CLAUDE.md to version 2.2 with accurate line counts and section breakdowns
- Updated line count to 1202 lines (current state)
- Updated line number breakdowns for CSS, HTML, and JavaScript sections
- Documented recent UI and content improvements from prior merges

### Recent Updates (Prior Merges)
- **Angina Prompt Update**: Updated angina prompt wording to clarify "recent or ongoing chest pain" and explicitly documented anginal equivalents
- **ACS Section Update**: Modified ACS section text to list STEMI, NSTEMI, and Unstable Angina on separate lines for better readability
- **Orange Section Headings**: Added orange-themed section headings to all 7 decision tree steps for improved visual hierarchy
- **Tooltip Fix**: Fixed tooltip positioning to appear next to label text instead of at edge of container
- **Angina Question Update**: Updated Angina question text to clarify patient presentation and format history criteria as a list

### 2025-11-22
- Changed question tab left border color from blue to orange (#FF6720)
- Updated visual design for better brand consistency

### Previous
- Set up GitHub Pages deployment with GitHub Actions
- Created initial Claude integration guide (claude.md)
- Initial application development with 7-step decision tree

---

## Future Enhancement Ideas

### High Priority
1. **Data Persistence**: LocalStorage to save/recover selections on browser refresh
2. **Print Functionality**: Generate clean printable summary of selections
3. **Accessibility**: WCAG 2.1 AA compliance (keyboard navigation, screen readers, ARIA labels)

### Medium Priority
4. **Export to PDF**: Client-side PDF generation of selection summary
5. **Analytics**: Privacy-compliant usage tracking (no PHI)
6. **Multi-language Support**: i18n for Spanish, other languages
7. **Keyboard Shortcuts**: Power user features (arrow keys to navigate, Enter to select)

### Low Priority
8. **Dark Mode**: User preference for light/dark theme
9. **Backend Integration**: API to connect with EMR/EHR systems
10. **Mobile App**: Native iOS/Android version using WebView
11. **Audit Trail**: Log selection history for compliance (requires backend)
12. **Offline PWA**: Progressive Web App with service worker for offline use

---

## Integration with EMR/EHR Systems

If you're tasked with integrating this application with external systems:

### Input Integration
- Application can accept URL parameters to pre-populate selections
- Example: `?acs=24hrs&stemi=yes` could pre-select ACS ≤ 24 hrs and STEMI

### Output Integration
- Current: User manually transcribes from summary
- Future: Could add "Copy to Clipboard" button or API endpoint to send data

### Security Considerations
- Application processes no PHI (Protected Health Information)
- All logic runs client-side in browser
- No data sent to external servers
- Safe for use in healthcare environments

---

## Contact & Support

### For Clinical Questions
- Consult NCDR CathPCI registry documentation
- Verify decision logic with clinical staff
- Contact Carta Healthcare for clinical validation

### For Technical Issues
- Check browser console for JavaScript errors
- Review validation error messages in UI
- Verify browser compatibility (modern browsers only)

### For Feature Requests
- Document the use case and clinical justification
- Consider whether feature aligns with "simple and portable" design philosophy
- Open GitHub issue or create pull request

---

## Appendix: Complete Function Reference

### State Management Functions
- `selectIndication(indication, elementId)` - Add indication to selection set
- `removeIndication(indication)` - Remove indication from selection set
- `updateSummary()` - Rebuild summary display
- `resetForm()` - Clear all selections

### Validation Functions
- `validateSelections()` - Check for incompatible combinations
- `displayErrors(errors)` - Show validation errors

### PCI Determination Functions
- `determinePCIIndication()` - Calculate PCI indication based on priority
- `showPCISubQuestion(questionType)` - Display conditional sub-questions
- `handlePCISubAnswer(questionType, answer)` - Process sub-question responses
- `selectPCIOption(indication)` - Select specific PCI indication
- `setPCIIndication(indication)` - Set final PCI indication

### UI Update Functions
- `updateArrows()` - Update all step completion arrows
- `updateArrowState(stepNum, isComplete)` - Update single step arrow
- `toggleRadioGroup(groupName, value)` - Handle radio button selection
- `handleRadioChange(groupName, value)` - Process radio button change
- `toggleCheckbox(checkboxId)` - Handle checkbox selection
- `handleCheckboxChange(indication, checkbox)` - Process checkbox change

### Helper Functions
- `showReminder(indication)` - Display contextual reminder
- `hideReminder(indication)` - Hide contextual reminder

---

## Glossary

- **ACS**: Acute Coronary Syndrome
- **CAD**: Coronary Artery Disease
- **Cath Lab**: Catheterization Laboratory
- **LV**: Left Ventricular
- **NCDR**: National Cardiovascular Data Registry
- **NSTE-ACS**: Non-ST-Elevation Acute Coronary Syndrome
- **PCI**: Percutaneous Coronary Intervention
- **PHI**: Protected Health Information
- **STEMI**: ST-Elevation Myocardial Infarction

---

*This document is maintained by AI assistants working on the CathPCI Decision Tree project. When making significant changes, please update the "Last Updated" date and increment the version number.*
