# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

This is a single-page marketing landing page for Levitate's "Say Goodbye to DIY Marketing" campaign. The site is built with vanilla HTML, CSS, and JavaScript - no build tools or framework dependencies.

**Live URL**: https://say-goodbye.levitate.ai (configured via CNAME)

## Architecture

### Single-File Application
- **index.html**: Contains all HTML markup, CSS styles (in `<style>` tag), and JavaScript (in `<script>` tag)
- No build process, bundler, or compilation required
- Deploy by pushing to the `main` branch (GitHub Pages)

### Key Technologies
- **Animations**: DotLottie Player (loaded via CDN: `@dotlottie/player-component@2.7.12`)
- **Fonts**: Google Fonts (Lexend, Sora, Sriracha)
- **Analytics**:
  - HubSpot tracking (ID: 4465199)
  - RB2B tracking (key: 4N210HQ5MZ6Z)
  - Google Analytics 4 (ID: G-3VCZFMJ9QD)

### CSS Architecture
The CSS uses a custom design system with CSS variables defined in `:root`:
- Brand colors: `--lev-blue`, `--lev-light-blue`, `--lev-dark-blue`, `--lev-light-green`, `--lev-med-green`, `--lev-yellow`, `--lev-salmon`, `--lev-purple`, `--lev-red`, `--lev-teal`
- Typography: Sora for headings (h1-h4), Lexend for body, Sriracha for emphasis text
- Signature visual pattern: "double-border" class creates Levitate's characteristic double-line border effect
- Responsive breakpoints: 991px, 767px, 600px, 500px

### JavaScript Features
1. **Count-up animations**: Stats animate on scroll using Intersection Observer
2. **Stats carousel**: Hero section rotates through 7 different statistics every 3.5 seconds
3. **Form handling**: Two forms submit to Zapier webhook
   - Demo form (with name, email, company, phone)
   - Email lead magnet form
   - Both include honeypot field (`department`) for bot prevention
   - Captures UTM parameters, page info, HubSpot user token (hutk), and IP address

### Asset Structure
```
/documents/          # Lottie animation files (.lottie)
/icons/             # SVG/PNG icons (18 files including specialist photos)
/images/            # Logos, awards, integration logos
/levitate-video.mp4 # Hero video (29MB)
```

## Development Workflow

### Local Development
Since this is a static HTML file with no build process:
```bash
# Serve locally (any method works)
python -m http.server 8000
# or
npx serve .
```

### Testing
- Open index.html directly in browser for quick edits
- Test form submissions (note: Zapier webhook uses `mode: 'no-cors'`)
- Test responsive breakpoints at: 991px, 767px, 600px, 500px
- Verify animations load (requires internet connection for CDN resources)

### Deployment
This repository uses GitHub Pages:
1. Push to `main` branch
2. GitHub Pages automatically deploys to https://say-goodbye.levitate.ai
3. CNAME file configures custom domain

**Deployment command**: `git push origin main`

## Key Implementation Patterns

### Form Submissions
Forms POST to: `https://hooks.zapier.com/hooks/catch/11261250/ufclj1v/`

Each submission includes:
- Form data (email, name, company, phone)
- UTM tracking parameters from URL
- HubSpot user token from cookies
- User IP address (fetched from ipify.org)
- Page metadata (URL, title, ID)
- Timestamp

### Animations
Lottie animations are loaded via:
```html
<dotlottie-player src="documents/[name].lottie" background="transparent" speed="1" loop autoplay></dotlottie-player>
```

Available animations:
- `home-hero-bg.lottie` - Hero section background
- `divider.lottie` - Hero section bottom divider
- `cta-divider.lottie` - CTA section top divider
- `reply-rate.lottie`, `animated-screenshot-loop.lottie` - Not currently used in index.html

### Color System
When adding new sections, use the established color classes:
- Borders: `.double-border.blue`, `.light-blue`, `.light-green`, `.salmon`, `.purple`
- Emphasis text: `.emphasis-text.yellow`, `.blue`, `.green`, `.red`
- Buttons: `.btn-primary`, `.btn-primary.hero-cta`, `.btn-primary.secondary`, `.btn-primary.green-light-hover`

### Responsive Design
The layout is mobile-first and uses:
- Flexbox for most layouts (two-column blocks, hero CTAs, footer)
- CSS Grid for card layouts (features, testimonials, integrations)
- `clamp()` for fluid typography (e.g., `font-size: clamp(40px, 6vw, 80px)`)

## Important Notes

### Tracking & Analytics
- All tracking pixels are production-ready and active
- HubSpot, RB2B, and GA4 track page views automatically
- Form submissions send data to Zapier, which likely routes to HubSpot CRM

### Content Updates
When updating copy:
- Hero stats are hardcoded in data attributes (e.g., `data-count="7939"`)
- Testimonials include attribution with name initials for avatars
- Industry tags use specific classes: `.financial`, `.legal`, `.nonprofit`, `.insurance`, `.other`

### Performance Considerations
- Video file is 29MB (levitate-video.mp4) - largest asset
- No image optimization or lazy loading implemented
- Lottie animations loaded from local `/documents/` directory
- Fonts loaded from Google Fonts CDN
- DotLottie player loaded from unpkg CDN

### Browser Compatibility
- Uses modern JavaScript (async/await, IntersectionObserver, fetch)
- CSS uses CSS Grid, Flexbox, CSS Variables
- Target: Modern browsers (Chrome, Firefox, Safari, Edge)
