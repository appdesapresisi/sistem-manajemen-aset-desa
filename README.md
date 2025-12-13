# 📦 Sistem Manajemen Aset Desa Mengwi

**Web-based asset management system untuk Desa Mengwi dengan fitur CRUD, approval workflow, dan export laporan.**

---

## 🎯 Tujuan & Overview

Sistem ini dirancang untuk mengelola inventaris/aset desa secara digital, memudahkan:
- ✅ Input & maintenance data aset dengan field dinamis
- ✅ Approval workflow oleh sekretaris desa
- ✅ Export laporan (PDF/Excel) dengan tanda tangan pejabat
- ✅ Dashboard statistik & tracking logs
- ✅ Role-based access (Super Admin, Admin/Sekretaris, Kepala Urusan)

**Target Users:**
- 👤 **Super Admin**: Manage konfigurasi sistem, users, pejabat, jenis aset
- 👤 **Sekretaris Desa**: Input/approval inventaris, export laporan
- 👤 **Kepala Urusan**: Input data aset, lihat laporan

---

## 🛠️ Tech Stack

### **Frontend (Current - Template Stage)**
| Teknologi | Fungsi |
|-----------|--------|
| **HTML5** | Semantic markup dengan template inheritance |
| **Tailwind CSS** | Utility-first CSS framework (10 color presets) |
| **SCSS** | Component styling & customization |
| **Gulp 4** | Build automation & task runner |
| **Feather Icons** | Icon library untuk UI |

**Theme Features:**
- 10 color presets (preset-1 hingga preset-10)
- 4 layout options (vertical, horizontal, compact, tab)
- Dark/Light mode support
- Responsive design (mobile, tablet, desktop)

### **Backend**
| Teknologi | Fungsi |
|-----------|--------|
| **Laravel 11** | PHP framework untuk REST API & Blade views |
| **MySQL 8** | Relational database |
| **Eloquent ORM** | Model relationships & JSON casting |
| **Blade Templates** | Server-side templating |

**Features:**
- Authentication & RBAC (Role-Based Access Control)
- PDF/Excel export dengan DOMPDF & PhpSpreadsheet
- File upload (TTD pejabat, dokumen)
- Activity logging & audit trails
- JSON-based dynamic form fields (EAV pattern)

---

## 📊 Database Architecture

### **10 Tabel Utama:**

```
users ─────────┐
roles          ├─→ pejabats ─────────┐
               │                     ├─→ jenis_inventaris_pejabats
               └─────────────────────┘
                                     
jenis_inventaris ──→ jenis_inventaris_data
                 └→ jenis_inventaris_pejabats
                 └→ inventaris

inventaris ──→ inventaris_values (EAV)
           ├→ inventaris_approvals
           └→ export_logs

export_logs ←─→ users, pejabats
```

### **Field Highlights:**

**inventaris** (Core Table)
```sql
- id, jenis_inventaris_id
- nama_barang, kode_barang, nup
- tahun_perolehan, nilai_perolehan
- status (draft/pending/approved/rejected)
- pejabat_override (JSON - optional override per item)
- created_by, updated_by
```

**Dynamic Fields via EAV:**
- `jenis_inventaris_data`: Template field definition per jenis
- `inventaris_values`: Actual values stored separately

**Tracking:**
- `inventaris_approvals`: Approval history dengan note & timestamp
- `export_logs`: Siapa export apa kapan dengan pejabat mana

---

## 📁 Struktur Project

```
manajemen-aset-desa/
├── .github/
│   └── workflows/
│       └── deploy.yml              # GitHub Actions auto-deploy
│
├── src/
│   ├── html/
│   │   ├── pages/                  # Page templates
│   │   │   ├── login.html
│   │   │   ├── manajemen-aset.html (List)
│   │   │   ├── inventaris-form.html (Create/Edit)
│   │   │   ├── users.html, user-form.html
│   │   │   ├── pejabats.html, pejabat-form.html
│   │   │   ├── jenis-inventaris.html, jenis-inventaris-form.html
│   │   │   └── export-data.html
│   │   │
│   │   └── layouts/                # Reusable layout components
│   │       ├── head-page-meta.html (Meta tags & base href)
│   │       ├── head-css.html       (Stylesheet imports)
│   │       ├── layout-vertical.html (Main layout)
│   │       ├── sidebar.html
│   │       ├── topbar.html
│   │       └── footer-js.html      (Script includes)
│   │
│   └── assets/
│       ├── scss/                   # Custom component styles
│       │   ├── style.scss
│       │   └── themes/
│       ├── js/                     # JavaScript functionality
│       │   ├── script.js
│       │   ├── layout-*.js         (Layout variants)
│       │   └── admin/, forms/      (Page-specific scripts)
│       │
│       ├── images/                 # Static assets
│       │   ├── user/, pages/
│       │   └── layout/
│       │
│       └── fonts/                  # Icon & typography fonts
│           ├── feather.css
│           ├── fontawesome.css
│           ├── material.css
│           └── tabler-icons.min.css
│
├── dist/                           # BUILD OUTPUT (compiled)
│   ├── pages/
│   ├── assets/
│   └── index.html
│
├── tailwind_plugins/               # Custom Tailwind utilities
│   ├── badge.js, buttons.js
│   ├── card.js, dropdown.js
│   ├── forms.js, table.js
│   └── layouts/
│
├── gulpfile.js                     # Gulp 4 build configuration
├── tailwind.config.js              # Tailwind theme config
├── postcss.config.js               # PostCSS plugins
├── package.json                    # Dependencies & scripts
└── README.md                       # This file
```

---

## 🚀 Development & Deployment

### **Setup Local Development**

1. **Clone & Install:**
```bash
git clone https://github.com/appdesapresisi/sistem-manajemen-aset-desa.git
cd sistem-manajemen-aset-desa
npm install
```

2. **Development Mode (Watch & Hot Reload):**
```bash
npm start
# Akses: http://localhost:3000
# Auto-reload saat edit src/ files
```

3. **Production Build:**
```bash
npm run build
# Generate optimized dist/ folder
# HTML minified, CSS/JS minified & compressed
```

4. **Format Code (Tailwind class sorting):**
```bash
npm run format
```

### **Deployment ke GitHub Pages**

**Automatic via GitHub Actions:**
- File: `.github/workflows/deploy.yml`
- Trigger: Push ke `main` atau `master` branch
- Proses: Auto-build → publish `dist/` → live in 1-2 menit
- URL: **https://appdesapresisi.github.io/sistem-manajemen-aset-desa**

**Base Configuration:**
- `<base href="/sistem-manajemen-aset-desa/">` di `head-page-meta.html`
- Semua asset paths otomatis adjust
- Routing (pages linking) bekerja seamless

---

## 📋 Pages & Fitur (Current Template)

| Page | URL | Role | Fungsi |
|------|-----|------|--------|
| **Login** | `/pages/login.html` | Public | Autentikasi user |
| **Dashboard** | `/dashboard/index.html` | All | KPI, charts, recent activity |
| **Manajemen Aset** | `/pages/manajemen-aset.html` | Admin | List inventaris, filter, search |
| **Tambah/Edit Aset** | `/pages/inventaris-form.html` | Admin | Dynamic form per jenis |
| **Users** | `/pages/users.html` | Super Admin | Manage users & roles |
| **Pejabat** | `/pages/pejabats.html` | Super Admin | Manage officials + upload TTD |
| **Jenis Inventaris** | `/pages/jenis-inventaris.html` | Super Admin | Manage asset categories & fields |
| **Export Data** | `/pages/export-data.html` | Admin | PDF/Excel export with signatures |

### **Key Features (Frontend - Ready):**
✅ Responsive layout (sidebar, topbar, breadcrumb)
✅ Theme customization (10 presets + dark mode)
✅ Form components (input, select, textarea, file upload)
✅ Data tables dengan sorting/filter
✅ Modal dialogs untuk actions
✅ Icon system (Feather, FontAwesome, Tabler)

---

## 🔧 Build System (Gulp 4)

### **Gulp Tasks:**

```bash
gulp              # Default: Watch files, serve on localhost:3000
gulp build-prod   # Production: Minify HTML/CSS/JS, optimize images
```

### **Processing Pipeline:**

```
src/html/pages/*.html
    ↓ (File include with @@include syntax)
    ↓ (Compile via gulp-file-include)
    ↓
dist/pages/*.html

src/assets/scss/*.scss
    ↓ (SCSS → CSS)
    ↓ (Tailwind processing)
    ↓ (Autoprefixer)
    ↓ (Minify in prod)
    ↓
dist/assets/css/style.css

src/assets/js/*.js
    ↓ (Babel transpilation ES6→ES5)
    ↓ (Minify in prod)
    ↓
dist/assets/js/script.js
```

### **Theme Configuration (gulpfile.js):**
```javascript
const preset_theme = 'preset-1';    // Color scheme
const theme_layout = 'vertical';    // Layout type
const dark_layout = 'false';        // Dark/Light mode
const sidebar_theme = 'dark';       // Sidebar colors
```

---

## 🔐 Security & Best Practices

### **Frontend (Current):**
- ✅ Relative paths untuk asset portability
- ✅ CSP-friendly (no inline scripts)
- ✅ Responsive & accessible HTML
- ✅ Never edit `dist/` directly (auto-generated)

### **Backend (Planned):**
- 🔒 Password hashing (bcrypt)
- 🔒 CSRF protection
- 🔒 Rate limiting
- 🔒 Input validation & sanitization
- 🔒 Role-based authorization checks
- 🔒 Audit logging untuk sensitive actions

---

## 📚 Next Phase: Backend Implementation

### **Planned Deliverables:**

1. **Laravel Migrations & Models**
   - All 10 tables dengan relationships
   - JSON casting untuk pejabat_override
   - Timestamps & soft deletes

2. **API Endpoints**
   ```
   POST /api/inventaris              - Create
   GET  /api/inventaris              - List (with filter)
   GET  /api/inventaris/{id}         - Detail
   PATCH /api/inventaris/{id}        - Update
   DELETE /api/inventaris/{id}       - Delete
   
   POST /api/inventaris/{id}/approve - Approval action
   POST /api/export                  - Export PDF/Excel
   GET  /api/export-logs             - Log history
   ```

3. **Blade Templates**
   - Convert HTML template ke Blade
   - Dynamic form rendering per jenis
   - Dashboard queries & charts

4. **Export Functionality**
   - DOMPDF untuk PDF generation
   - PhpSpreadsheet untuk Excel
   - Auto-inject TTD image pejabat

5. **Authentication**
   - Laravel Breeze/Sanctum
   - Multi-role middleware

---

## 📖 Development Workflow

### **Editing Pages:**
```bash
# 1. Edit HTML
vim src/html/pages/manajemen-aset.html

# 2. Save → Gulp auto-compiles & browser reloads
#    (Watch task running dari npm start)

# 3. Test locally
#    http://localhost:3000/pages/manajemen-aset.html

# 4. Commit & push
git add .
git commit -m "Update: Manajemen aset page styling"
git push origin main
# → GitHub Actions auto-build & deploy
```

### **Editing Styles:**
```bash
# Edit SCSS
vim src/assets/scss/themes/tailwind.scss

# Use Tailwind utilities in HTML
# Custom styles via tailwind_plugins/

# Build & test
npm start
```

---

## 🤝 Kontribusi & Timeline

### **Phase 1: Template & Setup** ✅ DONE
- Setup Gulp build system
- Template pages design
- GitHub Pages configuration

### **Phase 2: Backend**
- Laravel migrations & models
- API endpoints
- Authentication
- Database seeders

### **Phase 3: Features**
- Dynamic form fields
- Approval workflow
- Export with signatures
- Dashboard analytics

### **Phase 4: Testing & Deploy**
- Unit & integration tests
- Performance optimization
- Production deployment

---

## 📞 Support & Contact

**Repository:** https://github.com/appdesapresisi/sistem-manajemen-aset-desa  
**Live Demo:** https://appdesapresisi.github.io/sistem-manajemen-aset-desa

---

## 🎓 Documentation References

- [Tailwind CSS Docs](https://tailwindcss.com)
- [Gulp 4 Docs](https://gulpjs.com)
- [Datta Able Template](https://codedthemes.gitbook.io/datta-able)
- [Laravel 11 Docs](https://laravel.com/docs)

---

**Last Updated:** December 12, 2025  