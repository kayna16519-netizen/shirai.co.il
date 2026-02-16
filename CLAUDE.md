# CLAUDE.md

Guidelines for AI assistants working on the shirai.co.il codebase.

## Project Overview

**shirai.co.il** is a static Hebrew (RTL) landing page for "שיר קידן" (Shir Kaydan), offering administrative management services for businesses. The site captures leads through a contact form.

- **Language**: Hebrew (`lang="he"`, `dir="rtl"`)
- **Stack**: Vanilla HTML5, CSS3, JavaScript (no build tools, no package manager)
- **Domain**: shirai.co.il

## Repository Structure

```
shirai.co.il/
├── index.html              # Main landing page (lead capture form, all sections)
├── thank-you.html          # Post-submission confirmation page
├── shir.png                # Hero image (768x1344 PNG)
├── index.html.docx         # Word document backup of index.html
├── thank-you.html .docx    # Word document backup of thank-you.html
└── README.md               # Minimal project readme
```

## Technical Details

### index.html (Main Page)

- **Styling**: All CSS is embedded in a `<style>` block (no external stylesheets)
- **JavaScript**: Inline `<script>` at bottom of file, vanilla JS only
- **CSS variables** define the theme:
  - `--bg: #f7f3ec` (warm off-white background)
  - `--accent: #0f3b2e` (dark green)
  - `--accent2: #2b4a66` (smoky blue)
  - `--text: #14202b` (blue-gray text)
  - `--muted: #5b6b79` (secondary text)
- **Sections**: Header, Hero with lead form, How It Works, Services, Before/After, Target Audience, Footer CTA
- **Form**: Submits name, phone, and optional message. Currently uses `preventDefault()` with a placeholder for webhook integration (Make/Zapier)

### thank-you.html (Confirmation Page)

- **Styling**: Uses Tailwind CSS via CDN (`cdn.tailwindcss.com`)
- **Fonts**: Heebo (body) and Rubik (headings) from Google Fonts
- **Icons**: Font Awesome 6.4.0 via CDN
- **Brand colors** (different from main page):
  - Primary: `#7c3aed` (violet)
  - Secondary: `#db2777` (pink)
  - Dark: `#2e1065` (deep violet)

## Development Conventions

### Code Style

- All CSS and JS are embedded inline within HTML files (no separate `.css` or `.js` files)
- CSS uses custom properties (variables) for theming on `index.html`
- Class names follow BEM-inspired kebab-case (e.g., `.hero-copy`, `.form-card`, `.cta-strip`)
- Responsive design via `@media (max-width: ...)` breakpoints
- Hebrew comments in code (e.g., `/* לבן חם / אוף-וייט */`)

### Accessibility

- `aria-label` attributes on interactive and landmark elements
- `aria-live="polite"` on the toast notification
- `aria-hidden="true"` on decorative elements
- Form inputs include `autocomplete` attributes
- Semantic HTML5 elements (`<header>`, `<main>`, `<section>`, `<footer>`)

### RTL Considerations

- Root `<html>` has `dir="rtl"` and `lang="he"`
- Layout and text flow are right-to-left
- When modifying CSS layout properties, always test RTL alignment
- Phone number input uses `dir="ltr"` for proper digit display

## Build & Deployment

- **No build step** -- files are served as-is (static hosting)
- **No package manager** -- no `package.json`, no `node_modules`
- **No tests** -- no test framework or test files
- **No linting** -- no ESLint, Prettier, or formatting tools configured
- Deploy by uploading static files to any hosting provider (Vercel, Netlify, S3, etc.)

## Known Placeholders (Action Required for Production)

These items in the code need real values before going live:

1. **Form action** (`index.html`): Form submission is currently intercepted with `preventDefault()`. Replace with a real webhook URL (Make, Zapier, or custom endpoint) -- see comment at line ~502
2. **Contact info** (`index.html:469`): Footer uses placeholder values `050-0000000` and `hello@example.com`
3. **Form redirect**: After real form submission, redirect to `thank-you.html`

## Guidelines for Making Changes

1. **Read before editing** -- always read the full file before making modifications
2. **Preserve RTL** -- all new content must respect right-to-left layout
3. **Keep it static** -- do not introduce build tools, frameworks, or package managers unless explicitly requested
4. **Embedded styles** -- add CSS to the existing `<style>` block in `index.html`; do not create separate CSS files
5. **Maintain accessibility** -- add `aria-*` attributes to new interactive elements
6. **Hebrew content** -- all user-facing text should be in Hebrew unless specified otherwise
7. **Image optimization** -- `shir.png` is 1.3 MB; consider optimizing if adding more images
8. **Style consistency** -- use existing CSS variables (`--accent`, `--text`, etc.) rather than hardcoded colors
9. **No breaking changes** -- the `.docx` files are reference backups; do not delete them

## Git Workflow

- **Primary branch**: `main` (remote) / `master` (local)
- Commit messages have been in English so far
- No branch protection rules or CI/CD pipelines configured
