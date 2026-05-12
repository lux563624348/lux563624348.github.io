# To Be Improved — Xiang Li Personal Website

This document summarizes issues and recommendations identified during a codebase review of the personal portfolio website.

---

## 🔴 Critical

### 1. Google Maps API Key Missing
- **File**: All HTML pages (`index.html`, `about.html`, `contact.html`, `travel.html`, `photography.html`, `presentation.html`)
- **Issue**: `<script src="https://maps.googleapis.com/maps/api/js?key=YOUR_API_KEY_HERE&sensor=false">` uses a placeholder key. This causes the map to fail silently with a loading error.
- **Fix**: Replace `YOUR_API_KEY_HERE` with a valid Google Maps API key, or remove the map from pages that don't need it.

### 2. Contact Form Has No Handler
- **File**: `contact.html`
- **Issue**: The contact form has `action="#"` — no backend endpoint or JavaScript to actually submit the form data. Visitors who fill it out will see nothing happen.
- **Fix**: Use a free form backend service like [Formspree](https://formspree.io), [EmailJS](https://www.emailjs.com), or [Netlify Forms](https://www.netlify.com/products/forms/) (if hosted on Netlify).

### 3. Missing JavaScript Files (404 Errors)
- **Files Referenced (not found in `js/` directory)**: `jquery.min.js`, `jquery-migrate-3.0.1.min.js`, `popper.min.js`, `bootstrap.min.js`, `jquery.waypoints.min.js`, `jquery.stellar.min.js`, `owl.carousel.min.js`, `jquery.magnific-popup.min.js`, `jquery.animateNumber.min.js`, `jquery.timepicker.min.js`, `scrollax.min.js`
- **Issue**: Every HTML page loads many JS files that don't exist in the repository. These will all return 404 errors in the browser console, and the associated functionality will be broken.
- **Fix**: Either add the actual files to `js/`, or switch to CDN links (recommended — faster load times, caching benefits, fewer files to maintain).

### 4. Bootstrap CSS Loaded Three Times
- **Files**: `css/style.css` (contains full Bootstrap CSS inline ~230KB), `css/bootstrap/` (separate Bootstrap files directory), `scss/style.scss` (imports Bootstrap via SCSS)
- **Issue**: The compiled `style.css` already includes the complete Bootstrap CSS embedded within it, plus there are separate Bootstrap CSS files, and the SCSS source also imports Bootstrap. This is massive redundant bloat.
- **Fix**: Strip Bootstrap from `style.css` and load it as a separate minified file via CDN, or keep only custom styles in `style.css`.

---

## 🟡 Medium Priority

### 5. About Page Is Almost Empty
- **File**: `about.html`
- **Issue**: The page only has a hero section with the same tagline as the homepage. No bio, education timeline, research interests, publications list, or any actual "about me" content below the hero.
- **Fix**: Add sections for biography, education, research interests, publications, awards, etc.

### 6. Placeholder / Filler Content
- **File**: `index.html`
- **Issue**: The homepage has 12 blog/project cards. Only 3 describe Xiang Li's actual work (ML, Immunology, Physics teaching). The remaining 9 are generic Colorlib template filler (author "Dave Lewis", topics like "Fashion", generic travel posts). Most are commented out but still clutter the source.
- **Fix**: Remove all commented-out filler cards. Replace with real projects, publications, or remove entirely.

### 7. Photography Page Uses Generic Data
- **File**: `photography.html`
- **Issue**: All 12 photos use generic filenames (`image_1.jpg` through `image_12.jpg`) and have non-specific titles like "Blunt Dawn", "Liquid Pearl", "Great Fall". It's unclear whether these are actual photographs or placeholder images.
- **Fix**: Replace with real photography if available, with proper titles, descriptions, and meaningful filenames.

### 8. Navigation Inconsistency
- **File**: `travel.html`
- **Issue**: The navigation menu in travel.html lists items in a different order (`Home → Resume → Photography → Travel → About → Contact`) compared to other pages (`Home → Resume → Presentation → Photography → About → Contact`). The Presentation page is missing from travel's nav.
- **Fix**: Make navigation consistent across all pages.

### 9. About Page Social Links Are Broken
- **File**: `about.html`
- **Issue**: The hero section social links use `href="#"` instead of actual profile URLs. The sidebar footer also uses `#` for all social links.
- **Fix**: Replace with real URLs matching other pages (e.g., `https://twitter.com/xiangli`, `https://linkedin.com/in/xiangli-lucky-guy`).

### 10. Missing Favicon
- **Files**: All HTML pages
- **Issue**: No `<link rel="icon">` tag exists in any page header. Browser tabs will show a generic icon.
- **Fix**: Create or add a favicon and include it in the `<head>` of every page.

### 11. "Scroll Down" Link Does Nothing
- **File**: `index.html`
- **Issue**: The "Scroll Down For More About Me" button links to `href="#"` — clicking it doesn't scroll to any content. The projects section below is the intended target.
- **Fix**: Add an anchor `<section id="projects">` and link to `#projects`, or use a smooth scroll script.

---

## 🟢 Minor / Nice-to-Have

### 12. HTTPS for Google Maps
- **Files**: All HTML pages
- **Issue**: Google Maps script loads over `http://maps.googleapis.com` — should use `https://` to avoid mixed content warnings.
- **Fix**: Change to `https://maps.googleapis.com/maps/api/js?...`.

### 13. SCSS Primary Color Mismatch
- **File**: `scss/style.scss`
- **Issue**: The SCSS defines `$primary: #f05d23` (orange), but the compiled `css/style.css` uses `#78d5ef` (light blue) as the primary color. This means customizing via SCSS won't produce the expected output without recompiling.
- **Fix**: Either update the SCSS variable or ensure the build process (Prepros) is configured correctly and run after edits.

### 14. No Performance Optimization
- **Issue**: 16+ separate JS files loaded on every page (render-blocking). Large hero background images with no lazy loading or responsive `srcset`.
- **Fix**: Defer non-critical JS, combine/minify where possible, use responsive images with `<picture>` or `srcset`.

### 15. Weak SEO Metadata
- **Files**: All HTML pages
- **Issue**: Meta descriptions exist but are minimal. Missing: Open Graph image tag, Twitter card metadata, canonical URLs.
- **Fix**: Add `og:image`, `twitter:card`, and `<link rel="canonical">` to each page.

### 16. No Build System / Package Manager
- **File**: No `package.json` found
- **Issue**: The project relies on Prepros (a GUI tool) for SCSS compilation. There's no `package.json`, no modern build tooling (Webpack, Vite, etc.), and no dependency management.
- **Fix**: Add a `package.json` with build scripts for SCSS compilation, JS minification, and asset optimization.

### 17. Travel Content Authenticity
- **File**: `travel.html`
- **Issue**: Travel posts describe trips (Grand Canyon, NYC, Alaska, Tokyo, Swiss Alps) but use placeholder images (`image_1.jpg` etc.). It's not clear if these are real travel experiences or template content.
- **Fix**: Replace with real travel photos and stories, or remove the page if not applicable.

---

## 📋 Priority Action Plan

| Priority | Task | Effort | Impact |
|----------|------|--------|--------|
| 1 | Replace `YOUR_API_KEY_HERE` with real Google Maps key (or remove API) | 10 min | 🔴 Broken feature |
| 2 | Add missing JS files via CDN links | 30 min | 🔴 Broken functionality |
| 3 | Set up contact form handler | 30 min | 🟡 Broken feature |
| 4 | Remove placeholder/filler content from index.html | 20 min | 🟡 Content quality |
| 5 | Build out About page with real bio | 1–2 hrs | 🟡 Content gap |
| 6 | Fix navigation consistency | 15 min | 🟡 UX consistency |
| 7 | Fix about.html social links | 5 min | 🟡 Broken links |
| 8 | Clean up duplicate Bootstrap CSS | 30 min | 🟢 Performance |
| 9 | Add favicon | 15 min | 🟢 Polish |
| 10 | Add Open Graph + Twitter meta tags | 15 min | 🟢 SEO |
