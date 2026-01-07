# UNIR Masterclass Landing Page

A modern, interactive landing page created for the Full Stack Developer Master's program at UNIR. This single-page application showcases a professional journey with smooth scrolling, animations, and bilingual support (English/Spanish).

## 📋 Table of Contents

- [Features](#-features)
- [Prerequisites](#-prerequisites)
- [Installation](#-installation)
- [Development](#-development)
- [Building for Production](#-building-for-production)
- [Project Structure](#-project-structure)
- [Features Overview](#-features-overview)
- [Customization](#-customization)
- [Troubleshooting](#-troubleshooting)
- [License](#-license)

## ✨ Features

- 🎨 **Modern Design**: Clean, minimalist design with smooth animations
- 🌐 **Bilingual Support**: Toggle between English and Spanish
- 📱 **Fully Responsive**: Optimized for desktop, tablet, and mobile devices
- ⏱️ **Chronometer**: Track time spent on the presentation
- 🎯 **Smooth Scrolling**: Navigate between sections seamlessly
- 🖼️ **Interactive Elements**: Hover effects and animated components
- 📊 **Professional Timeline**: Visual representation of work experience
- 🎭 **3D Character Graphics**: Engaging visual elements throughout

## 🔧 Prerequisites

Before you begin, ensure you have the following installed on your system:

- **Node.js** (version 16.x or higher)
  - Download from [nodejs.org](https://nodejs.org/)
  - Verify installation: `node --version`
  
- **npm** (comes with Node.js) or **yarn**
  - Verify installation: `npm --version`

- **Git** (optional, for version control)
  - Download from [git-scm.com](https://git-scm.com/)

## 📦 Installation

Follow these steps to set up the project on your local machine:

### Step 1: Clone or Download the Repository

If you have the repository URL:
```bash
git clone <repository-url>
cd unir-masterclass-albertoglez
```

Or download and extract the ZIP file, then navigate to the project directory:
```bash
cd unir-masterclass-albertoglez
```

### Step 2: Install Dependencies

Install all required packages using npm:

```bash
npm install
```

This command will:
- Read the `package.json` file
- Download and install Vue 3, Vite, and all other dependencies
- Create a `node_modules` folder with all packages

**Expected output:**
```
added 50 packages, and audited 51 packages in 5s
```

### Step 3: Verify Installation

Check that everything is installed correctly:

```bash
npm list --depth=0
```

You should see Vue and Vite listed in the dependencies.

## 🛠️ Development

### Step 1: Start the Development Server

Run the development server:

```bash
npm run dev
```

**Expected output:**
```
  VITE v5.x.x  ready in xxx ms

  ➜  Local:   http://localhost:5173/
  ➜  Network: use --host to expose
```

### Step 2: Open in Browser

1. Open your web browser
2. Navigate to `http://localhost:5173`
3. You should see the landing page

### Step 3: Development Features

The development server includes:
- **Hot Module Replacement (HMR)**: Changes are reflected instantly without page refresh
- **Fast Refresh**: Vue components update in real-time
- **Error Overlay**: Errors are displayed directly in the browser

### Step 4: Make Changes

1. Edit files in the `src/` directory
2. Save your changes
3. See updates automatically in the browser

### Step 5: Stop the Server

Press `Ctrl + C` (or `Cmd + C` on Mac) in the terminal to stop the development server.

## 🏗️ Building for Production

### Step 1: Create Production Build

Generate optimized production files:

```bash
npm run build
```

**What happens:**
- Vite compiles and minifies all code
- Assets are optimized
- Files are generated in the `dist/` folder

**Expected output:**
```
dist/index.html                   0.xx kB
dist/assets/index-xxxxx.js        xx.xx kB
dist/assets/index-xxxxx.css       x.xx kB
```

### Step 2: Preview Production Build

Test the production build locally:

```bash
npm run preview
```

This starts a local server with the production build:
```
  ➜  Local:   http://localhost:4173/
```

### Step 3: Deploy

The `dist/` folder contains all files needed for deployment. Upload its contents to:

- **Static Hosting**: Netlify, Vercel, GitHub Pages
- **Web Server**: Apache, Nginx
- **CDN**: Cloudflare, AWS CloudFront

## 📁 Project Structure

```
unir-masterclass-albertoglez/
│
├── public/                      # Static assets
│   └── favicon.jpg             # Site favicon
│
├── src/                         # Source files
│   ├── assets/                 # Images and fonts
│   │   ├── fonts/              # Custom fonts (Switzer)
│   │   ├── *.png               # Character and decorative images
│   │   └── *.svg               # Logo files
│   │
│   ├── components/             # Vue components
│   │   └── LandingPage.vue     # Main landing page component
│   │
│   ├── App.vue                 # Root Vue component
│   ├── main.js                 # Application entry point
│   └── style.css               # Global styles and font definitions
│
├── index.html                   # HTML template
├── vite.config.js              # Vite configuration
├── package.json                 # Project dependencies and scripts
└── README.md                    # This file
```

## 🎯 Features Overview

### Sections

1. **Masterclass** - Hero section with main title
2. **Who I am** - Personal introduction with interactive profile
3. **Where I've worked** - Professional timeline
4. **Why I studied** - Motivation and reasons
5. **When I studied** - Study experience highlights
6. **Master vs. Bootcamp** - Comparative analysis
7. **My First Job** - Career beginning story
8. **Soft Skills** - Key professional skills
9. **Resizes** - Current work/project showcase

### Interactive Elements

- **Language Toggle**: Switch between English/Spanish (top-right)
- **Side Navigation**: Quick access to all sections (right side)
- **Scroll Arrows**: Navigate to next section (bottom center)
- **Chronometer**: Time tracking (bottom-left)
- **Hover Effects**: Interactive profile and timeline elements
- **Dropdown Menu**: Resizes section links

## 🎨 Customization

### Changing Content

1. **Text Content**: Edit translations in `src/components/LandingPage.vue`
   - Find the `translations` object (around line 548)
   - Modify English (`en`) or Spanish (`es`) text

2. **Images**: Replace files in `src/assets/`
   - Keep the same file names, or update imports in `LandingPage.vue`

3. **Colors**: Modify CSS variables in `src/components/LandingPage.vue`
   - Main color: `#1a1a2e`
   - Background: `whitesmoke`

### Adding New Sections

1. Add section HTML in `LandingPage.vue` template
2. Add section to `sections` computed property
3. Add translations for the section
4. Update navigation automatically

### Styling Changes

- Main styles: `src/components/LandingPage.vue` (in `<style>` section)
- Global styles: `src/style.css`
- Responsive breakpoints: 1024px, 768px, 480px

## 🐛 Troubleshooting

### Issue: `npm install` fails

**Solution:**
```bash
# Clear npm cache
npm cache clean --force

# Delete node_modules and package-lock.json
rm -rf node_modules package-lock.json

# Reinstall
npm install
```

### Issue: Port 5173 already in use

**Solution:**
```bash
# Use a different port
npm run dev -- --port 3000
```

### Issue: Changes not reflecting

**Solution:**
1. Stop the dev server (Ctrl+C)
2. Clear browser cache
3. Restart: `npm run dev`
4. Hard refresh: Ctrl+Shift+R (or Cmd+Shift+R on Mac)

### Issue: Build fails

**Solution:**
```bash
# Check Node.js version (should be 16+)
node --version

# Update dependencies
npm update

# Try building again
npm run build
```

### Issue: Images not loading

**Solution:**
- Verify image paths in `src/assets/`
- Check import statements in `LandingPage.vue`
- Ensure file names match exactly (case-sensitive)

### Issue: Fonts not loading

**Solution:**
- Verify font files exist in `src/assets/fonts/`
- Check `@font-face` declarations in `src/style.css`
- Ensure font file formats are correct (.woff2, .woff)

## 📝 Available Scripts

| Command | Description |
|---------|-------------|
| `npm run dev` | Start development server |
| `npm run build` | Build for production |
| `npm run preview` | Preview production build |

## 🌐 Browser Support

- Chrome (latest)
- Firefox (latest)
- Safari (latest)
- Edge (latest)
- Mobile browsers (iOS Safari, Chrome Mobile)

## 📄 License

This project is created for educational purposes as part of the UNIR Full Stack Developer Master's program.

## 👤 Author

**Alberto González**
- LinkedIn: [albegosu](https://www.linkedin.com/in/albegosu/)
- Company: Resizes

## 🙏 Acknowledgments

- UNIR for the Master's program
- Vue.js team for the amazing framework
- Vite team for the build tool

---

**Note**: This is a presentation project for academic purposes. For questions or issues, please refer to the project documentation or contact the author.
