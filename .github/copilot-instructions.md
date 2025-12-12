# AI Coding Agent Instructions

## Project Overview

**Manajemen Aset Desa** is a village asset management admin dashboard built on the **Datta Able Tailwind Admin Template**. It's a static HTML-based dashboard with compiled assets, designed for managing village assets and resources.

**Tech Stack:**
- HTML5 with template inheritance via `@@include()` (gulp-file-include)
- Tailwind CSS with custom theme presets (10 color schemes available)
- SCSS for component styling in `src/assets/scss/`
- Gulp 4 build system with Babel transpilation
- Custom Tailwind plugins in `tailwind_plugins/`
- PostCSS with autoprefixer for CSS compatibility

## Architecture

### Build Process (Gulp 4)
The build system transforms source files → compiled distribution:
- **Source:** `src/html/`, `src/assets/scss/`, `src/assets/js/`
- **Output:** `dist/` (served in production)
- **Commands:** `npm start` (dev watch), `npm run build` (production minified)

### Template Inheritance Pattern
Pages use `@@include()` to compose layouts from reusable fragments:

```html
<!-- In: src/html/pages/users.html -->
@@include('../layouts/head-page-meta.html', {'title': 'Users'})
@@include('../layouts/head-css.html')
@@include('../layouts/layout-vertical.html')
  <!-- Page content here -->
@@include('../layouts/footer-block.html')
@@include('../layouts/footer-js.html')
```

**Core layout files** in `src/html/layouts/`:
- `head-css.html` - Stylesheet links
- `head-page-meta.html` - Meta tags & title
- `layout-vertical.html` - Main layout structure (sidebar + topbar)
- `sidebar.html` - Navigation menu
- `topbar.html` - Header bar
- `loader.html` - Page loader animation
- `footer-js.html` - Script inclusions

### Tailwind Configuration
Theme customization via `gulpfile.js` configuration block (lines 25-50):
- `preset_theme` (preset-1 to preset-10) - Color schemes
- `theme_layout` (vertical, horizontal, compact, tab) - Layout variants
- `dark_layout` (true/false) - Dark mode
- `sidebar_theme` (dark/light) - Sidebar colors
- Custom spacing defined in `tailwind.config.js`: `sidebar-width: 264px`, `header-height: 74px`

Custom Tailwind plugins extend default utilities - check `tailwind_plugins/` for features like badges, buttons, cards, dropdowns, form components.

## Common Pages & Components

**Master Data Pages** (in `src/html/pages/`):
- `users.html` / `user-form.html` - User management
- `pejabats.html` / `pejabat-form.html` - Officials/staff management
- `jenis-inventaris.html` / `jenis-inventaris-form.html` - Asset category management
- `manajemen-aset.html` - Main asset dashboard with gradients
- `login.html` - Authentication page
- `form.html` - Generic form template

**Naming conventions:**
- List pages: `{resource}.html` (e.g., `users.html`)
- Form pages: `{resource}-form.html` (e.g., `user-form.html`)
- Redirect logic typically redirects back after action (e.g., save/delete button returns to list)

## Key Developer Workflows

### Build & Development
```bash
npm start          # Start Gulp watch (live reload on http://localhost:3000)
npm run build      # Production build: minify CSS/JS, minify HTML, compress images
npm run format     # Format code with Prettier (applies Tailwind class sorting)
```

### HTML Template Maintenance
When editing pages:
1. Changes in `src/html/` automatically trigger Gulp rebuild
2. Include parameters are passed as objects: `@@include('file.html', {key: 'value'})`
3. For new pages: copy existing page structure, update `@@include` paths and page title
4. Always ensure closing `@@include` tags for layout and footer blocks

### CSS Changes
- Global styles: `src/assets/scss/style.scss` imports `src/assets/scss/themes/tailwind.scss`
- Avoid direct CSS edits - use Tailwind classes in HTML
- When CSS utility is needed: add to `src/assets/scss/` and rebuild
- Tailwind plugins add custom components (e.g., `.btn-primary`, `.badge`)

### JavaScript
- Layout scripts: `src/assets/js/*.js` (executed on all pages)
- Page-specific scripts: `src/assets/js/{feature}/*.js` (e.g., admin/dashboard.js)
- Scripts included via `footer-js.html`
- Babel transpiles ES6+ to ES5 for browser compatibility

## Project-Specific Conventions

### Color System
Instead of hardcoding colors, use theme variables defined in `tailwind.config.js`:
- `text-theme-headings` (dark text for headings)
- `bg-theme-bodybg` (light page background)
- `dark:bg-themedark-cardbg` (dark mode card background)
- `bg-primary-500`, `bg-success-400` (semantic colors)

### Form Patterns
Forms in `src/html/pages/*-form.html` use:
- `.form-group` container classes
- Inline script handlers for submit/cancel (redirect logic)
- Example in `user-form.html`: form submission redirects to `users.html` on success

### Asset Management
Images and static files in `src/assets/`:
- Icons: Use Feather Icons CSS classes (included by `head-css.html`)
- Images: Stored in `src/assets/images/{category}/`
- Fonts: Located in `src/assets/fonts/` (FontAwesome, Material, Tabler included)

## CI/CD Pipeline

**GitHub Actions** (`.github/workflows/prod.yml`):
1. Triggered on: push to `master` or merged PR to `master`
2. Steps:
   - Checkout code
   - Install Node 20
   - Run `npm install && npm run build` 
   - Deploy `dist/` folder via SSH to production server (92.112.197.121)
3. **Important:** Builds automatically minify and optimize assets - never edit `dist/` directly

## Critical Notes

- **Always edit source files** in `src/`, never `dist/` (auto-generated)
- **Template includes are processed at build time** - no runtime templating
- **Paths in HTML are relative** - adjust for deployment context (e.g., `html/assets/css/` references)
- **Dark mode** is controlled by data attribute `data-pc-theme="dark"` on `<html>` element
- **New pages must follow inclusion pattern** to inherit layout, styling, and scripts

## Useful File References

- **Main config:** `gulpfile.js` (theme variables, build tasks)
- **Tailwind config:** `tailwind.config.js` (theme customization, spacing, colors)
- **Package scripts:** `package.json` (dev dependencies, build commands)
- **PostCSS:** `postcss.config.js` (Tailwind processing, autoprefixer, cssnano in prod)
- **Theme plugins:** `tailwind_plugins/` (custom Tailwind component extensions)
