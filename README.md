# NCDR CathPCI Decision Tree

An interactive web-based decision tree tool for medical professionals to help select appropriate "Cath Lab Indications" and "PCI Indications" based on patient characteristics and conditions.

## Overview

This application is designed for the National Cardiovascular Data Registry (NCDR) Catheterization/PCI program. It guides clinicians through a series of questions to determine the correct indications for catheterization procedures.

## Features

- **Interactive Decision Tree**: 7-step guided workflow for selecting cath lab indications
- **Real-time Validation**: Prevents selection of incompatible indications
- **Dynamic PCI Determination**: Automatically determines PCI indication based on selections
- **User-friendly Interface**: Clean, modern design with clear visual feedback
- **Helpful Reminders**: Context-sensitive guidance for specific selections

## Technology

This is a simple, self-contained static web application built with:
- HTML5
- CSS3
- Vanilla JavaScript
- No external dependencies or frameworks

## Deployment

### GitHub Pages (Recommended)

This project is automatically deployed to GitHub Pages using GitHub Actions.

**Live Site**: `https://CartaHealthcareCathPCI.github.io/cathpci-decision-tree/`

The deployment workflow:
1. Automatically triggers on push to the `main` branch
2. Deploys the static site to GitHub Pages
3. Site is available within minutes

### Manual Deployment

Since this is a static HTML file, you can deploy it anywhere:

1. **Any Web Server**: Copy `index.html` to your web server (Apache, Nginx, etc.)
2. **Netlify**: Drag and drop the file to Netlify
3. **Vercel**: Deploy directly from GitHub
4. **AWS S3**: Upload as a static website
5. **Local Testing**: Simply open `index.html` in any web browser

## Development

No build process required! To make changes:

1. Edit `index.html`
2. Refresh your browser to see changes
3. Commit and push to trigger automatic deployment

## Usage

1. Open the application in a web browser
2. Answer each question in the decision tree
3. View the automatically determined Cath Lab and PCI indications
4. Use the Reset button to start over

## Project Structure

```
cathpci-decision-tree/
├── index.html                       # Main application file
├── README.md                        # This file
└── .github/
    └── workflows/
        └── deploy-pages.yml         # GitHub Pages deployment workflow
```

## License

Copyright © Carta Healthcare / NCDR

## Support

For issues or questions, please contact the development team.
