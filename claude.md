# CathPCI Decision Tree - Claude Integration Guide

## Project Overview

This project is an interactive decision tree application for NCDR CathPCI (National Cardiovascular Data Registry Catheterization Percutaneous Coronary Intervention). It helps healthcare professionals determine the appropriate Cath Lab Indication and PCI Indication for cardiac catheterization procedures.

## Purpose

The decision tree guides clinicians through a series of questions to:
1. Identify appropriate **Cath Lab Indications** based on patient presentation
2. Automatically determine the corresponding **PCI Indication** based on the selected Cath Lab Indication

## Project Structure

```
cathpci-decision-tree/
├── README.md                          # Currently contains HTML application code
├── cathpci-decision-tree (1).html     # HTML application file
└── claude.md                          # This file - AI integration guide
```

## Technical Implementation

### Technology Stack
- **Frontend**: Pure HTML, CSS, and JavaScript (no frameworks)
- **Styling**: Gradient backgrounds, responsive design, modern UI components
- **State Management**: JavaScript Set and Object for tracking selections

### Key Features

1. **7-Step Decision Process**:
   - Step 1: ACS (Acute Coronary Syndrome) screening with timing
   - Step 2: Angina evaluation with history assessment
   - Step 3: CAD (Coronary Artery Disease) history
   - Step 4: Cardiac arrest screening
   - Step 5: Non-ischemic/structural issues (multiple selections)
   - Step 6: Non-cardiac/evaluation reasons
   - Step 7: Other indications (last resort)

2. **Validation Logic**:
   - Prevents incompatible indication combinations
   - Displays real-time validation errors
   - Enforces clinical decision rules

3. **Priority-Based PCI Determination**:
   - Uses highest priority Cath Lab Indication
   - Conditional sub-questions for specific scenarios
   - Automatic PCI Indication mapping

## Clinical Logic

### Indication Priority Order (Highest to Lowest)

1. ACS ≤ 24 hrs
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
17. Other

### PCI Indication Mapping

- **ACS ≤ 24 hrs** → Requires STEMI sub-question:
  - If STEMI: Multiple timing options (Immediate, Stable ≤12hrs, Stable >12hrs, Unstable, After lytics, Rescue)
  - If No: NSTE-ACS

- **ACS > 24 hrs** → Requires STEMI sub-question:
  - If STEMI: Timing options (Stable >12hrs, Unstable, After lytics, Rescue)
  - If No: NSTE-ACS

- **New Onset Angina ≤ 2 months** → Direct mapping

- **Stable Known CAD** → Requires angina sub-question:
  - If Yes: Stable angina
  - If No: CAD (without ischemic Sx)

- **All Others** → Other PCI Indication

## Development Guidelines

### When Making Changes

1. **Maintain Clinical Accuracy**: All validation rules are based on NCDR CathPCI registry requirements
2. **Preserve State Management**: The application uses `selectedIndications` Set and `pciSubAnswers` object
3. **Keep Validation Intact**: Don't remove validation rules without clinical justification
4. **Responsive Design**: Ensure changes work across different screen sizes

### Code Organization

- **Styling**: Lines 8-287 (CSS within `<style>` tags)
- **HTML Structure**: Lines 290-543 (Semantic sections for each step)
- **JavaScript Logic**: Lines 545-969 (State management, validation, PCI determination)

### Key Functions

- `selectIndication(indication, elementId)`: Adds indication to selection
- `removeIndication(indication)`: Removes indication
- `validateSelections()`: Checks for incompatible combinations
- `determinePCIIndication()`: Calculates appropriate PCI indication
- `showPCISubQuestion(questionType)`: Displays conditional questions
- `resetForm()`: Clears all selections

## Testing Scenarios

### Test Case 1: STEMI Patient
- Select: ACS ≤ 24 hrs → Yes → STEMI → Immediate PCI
- Expected PCI: STEMI - Immediate PCI for Acute STEMI

### Test Case 2: Stable CAD without Symptoms
- Select: CAD History → Yes → Stable Known CAD → No angina
- Expected PCI: CAD (without ischemic Sx)

### Test Case 3: Valvular Disease
- Select: Valvular Disease
- Expected: Reminder about including all stenosis/regurgitation
- Expected PCI: Other PCI Indication

### Test Case 4: Multiple Indications
- Select: Valvular Disease + LV Dysfunction + Suspected CAD
- Expected: All three appear in summary
- Expected PCI: Based on highest priority (Suspected CAD → Other PCI Indication)

## Common Issues and Solutions

### Issue: README.md contains HTML
**Current State**: The README.md file contains the full HTML application code instead of documentation.

**Recommendation**:
- Rename README.md to something like `cathpci-app.html` or `index.html`
- Create a proper README.md with project description, usage instructions, and deployment info

### Issue: Duplicate HTML File
**Current State**: Two HTML files with identical/similar content

**Recommendation**:
- Consolidate into a single, well-named HTML file
- Use version control for history rather than numbered copies

## Future Enhancement Ideas

1. **Data Persistence**: Save selections to localStorage for recovery
2. **Export Functionality**: Generate PDF/print summary of selections
3. **Analytics**: Track common indication patterns (privacy-compliant)
4. **Multi-language Support**: i18n for international use
5. **Backend Integration**: Connect to EMR/EHR systems
6. **Accessibility**: WCAG compliance improvements
7. **Mobile App**: Native iOS/Android version
8. **Audit Trail**: Log selection history for compliance

## Deployment

### Current Deployment
- Static HTML file (can be opened directly in browser)
- No server required
- No build process needed

### Production Deployment Options
1. **GitHub Pages**: Host directly from repository
2. **Netlify/Vercel**: Automatic deployments from git
3. **Hospital Intranet**: Deploy on internal web server
4. **Electron App**: Package as desktop application

## Contributing

When working on this project:
1. Test all validation logic thoroughly
2. Verify PCI indication mapping against clinical guidelines
3. Ensure backward compatibility with saved URLs/bookmarks
4. Maintain code comments for complex clinical logic
5. Update this documentation with significant changes

## Contact & Support

For questions about:
- **Clinical Logic**: Consult NCDR CathPCI documentation
- **Technical Issues**: Review validation errors and browser console
- **Feature Requests**: Document use case and clinical justification

---

*Last Updated: 2025-11-22*
*Version: 1.0*
