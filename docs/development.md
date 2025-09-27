# Development Guide

This guide covers the development process, build system, and deployment pipeline for the MkDocs React theme.

## 🛠️ Development Setup

### Prerequisites

Before starting development, ensure you have the following installed:

- **Python 3.11+**: Required for MkDocs
- **Node.js 18+**: Required for frontend build tools
- **Git**: Version control
- **Code Editor**: VS Code, WebStorm, or similar

### Environment Setup

1. **Clone the repository**:
```bash
git clone https://github.com/asap-programmer/static-site-python.git
cd static-site-python
```

2. **Create a virtual environment**:
```bash
python -m venv venv
source venv/bin/activate  # On Windows: venv\Scripts\activate
```

3. **Install Python dependencies**:
```bash
pip install -r requirements.txt
```

4. **Install Node.js dependencies**:
```bash
npm install
```

## 🏗️ Build System

### MkDocs Configuration

The site is configured through `mkdocs.yml`:

```yaml
site_name: My Docs
site_url: https://asap-programmer.github.io/static-site-pages
site_description: A modern static site built with MkDocs and custom React theme
site_author: Ilvir Nizaev

theme:
  name: null
  custom_dir: custom_theme
  static_templates:
    - 404.html

plugins:
  - search
  - minify:
      minify_html: true
      minify_js: true
      minify_css: true

markdown_extensions:
  - toc:
      permalink: true
  - codehilite:
      guess_lang: true
  - pymdownx.superfences
  - pymdownx.tabbed:
      alternate_style: true
```

### PostCSS Configuration

CSS processing is handled by PostCSS with the following configuration:

```javascript
module.exports = {
  plugins: [
    require('autoprefixer'),
    require('cssnano')({
      preset: 'default',
    }),
  ],
};
```

### Build Commands

| Command | Description |
|---------|-------------|
| `mkdocs serve` | Start development server |
| `mkdocs build` | Build static site |
| `npm run build:css` | Process CSS with PostCSS |
| `npm run watch:css` | Watch CSS files for changes |

## 🎨 Theme Development

### Theme Structure

```
custom_theme/
├── base.html              # Base template
├── main.html              # Main page template
├── 404.html               # Error page template
└── assets/
    ├── css/
    │   ├── custom.css     # Custom styles
    │   └── custom.min.css # Minified styles
    └── js/
        └── custom.js      # Custom JavaScript
```

### Template Development

#### Base Template (`base.html`)

The base template provides the overall page structure:

```html
<!DOCTYPE html>
<html lang="{{ lang or 'en' }}">
<head>
    <meta charset="utf-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>{% if page.title %}{{ page.title }} - {% endif %}{{ config.site_name }}</title>
    <!-- Additional head content -->
</head>
<body>
    <!-- Header -->
    <header>...</header>

    <!-- Main content -->
    <main>
        {% block content %}{% endblock %}
    </main>

    <!-- Footer -->
    <footer>...</footer>

    <!-- Scripts -->
    <script src="{{ 'assets/js/custom.js'|url }}"></script>
</body>
</html>
```

#### Main Page Template (`main.html`)

The main page template extends the base template with additional features:

```html
{% extends "base.html" %}

{% block extrahead %}
<style>
    .hero-gradient {
        background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
    }
</style>
{% endblock %}

{% block content %}
<!-- Hero Section -->
<section class="hero-gradient text-white py-20 mb-16">
    <!-- Hero content -->
</section>

<!-- Features Section -->
<section id="features" class="py-16">
    <!-- Features content -->
</section>

<!-- Content Section -->
<section class="py-16 bg-gray-50">
    <div class="bg-white rounded-lg shadow-sm p-8">
        {{ super() }}
    </div>
</section>
{% endblock %}
```

### CSS Development

#### Custom CSS (`custom.css`)

The custom CSS file includes:

- **Typography improvements**: Enhanced readability
- **Component styles**: Custom component styling
- **Animations**: Smooth transitions and effects
- **Responsive design**: Mobile-first approach
- **Print styles**: Optimized printing

#### PostCSS Processing

CSS is processed through PostCSS with:

- **Autoprefixer**: Automatic vendor prefixes
- **CSSnano**: CSS minification
- **Custom plugins**: Additional processing

### JavaScript Development

#### React Components

The theme includes several React components:

```javascript
// Theme Toggle Component
function ThemeToggle() {
    const [isDark, setIsDark] = useState(false);

    const toggleTheme = () => {
        setIsDark(!isDark);
        if (!isDark) {
            document.documentElement.classList.add('dark');
            localStorage.setItem('theme', 'dark');
        } else {
            document.documentElement.classList.remove('dark');
            localStorage.setItem('theme', 'light');
        }
    };

    return React.createElement('button', {
        onClick: toggleTheme,
        className: 'p-2 rounded-lg bg-gray-200 hover:bg-gray-300 transition-colors',
        'aria-label': 'Toggle theme'
    }, isDark ? '☀️' : '🌙');
}
```

#### Interactive Features

- **Mobile menu**: Responsive navigation
- **Search functionality**: Real-time search
- **Smooth scrolling**: Enhanced navigation
- **Code copying**: One-click code copying
- **Back to top**: Smooth scroll to top

## 🔄 Development Workflow

### Local Development

1. **Start the development server**:
```bash
mkdocs serve
```

2. **Open your browser** to `http://localhost:8000`

3. **Make changes** to templates, CSS, or JavaScript

4. **Refresh the browser** to see changes

### CSS Development

1. **Edit CSS files** in `custom_theme/assets/css/`

2. **Process with PostCSS**:
```bash
npm run build:css
```

3. **Watch for changes**:
```bash
npm run watch:css
```

### Testing

1. **Build the site**:
```bash
mkdocs build
```

2. **Validate HTML**:
```bash
html5validator --root site
```

3. **Test responsiveness** using browser dev tools

## 🚀 Deployment Pipeline

### GitHub Actions Workflow

The deployment pipeline includes:

1. **Testing Phase**:
   - HTML validation
   - Dependency installation
   - Build verification

2. **Build Phase**:
   - CSS processing with PostCSS
   - Static site generation
   - Asset minification

3. **Deployment Phase**:
   - GitHub Pages deployment
   - CDN integration
   - Cache configuration

### Workflow Configuration

```yaml
name: Build and Deploy MkDocs Site

on:
  push:
    branches:
      - main
  pull_request:
    branches:
      - main

jobs:
  test:
    runs-on: ubuntu-latest
    steps:
      - name: Checkout source repo
        uses: actions/checkout@v3

      - name: Set up Python
        uses: actions/setup-python@v4
        with:
          python-version: '3.11'

      - name: Install dependencies
        run: |
          python -m pip install --upgrade pip
          pip install -r requirements.txt

      - name: Validate HTML
        run: |
          mkdocs build
          pip install html5validator
          html5validator --root site --format text

  build-deploy:
    needs: test
    runs-on: ubuntu-latest
    if: github.ref == 'refs/heads/main'

    steps:
      # ... build and deployment steps
```

## 🧪 Testing

### Automated Testing

- **HTML Validation**: W3C HTML validation
- **CSS Validation**: CSS linting and validation
- **JavaScript Testing**: ESLint and functionality tests
- **Performance Testing**: Lighthouse CI integration

### Manual Testing

- **Cross-browser Testing**: Chrome, Firefox, Safari, Edge
- **Device Testing**: Mobile, tablet, desktop
- **Accessibility Testing**: Screen reader and keyboard navigation

### Testing Commands

```bash
# HTML validation
html5validator --root site

# CSS validation
npm run lint:css

# JavaScript testing
npm run test:js

# Performance testing
npm run lighthouse
```

## 📦 Dependencies

### Python Dependencies

```txt
mkdocs>=1.5.0
mkdocs-material>=9.0.0
mkdocs-minify-plugin>=0.7.0
pymdown-extensions>=10.0.0
```

### Node.js Dependencies

```json
{
  "devDependencies": {
    "autoprefixer": "^10.4.16",
    "cssnano": "^6.0.1",
    "html-minifier-terser": "^7.2.0",
    "postcss": "^8.4.32",
    "postcss-cli": "^11.0.0"
  }
}
```

## 🔧 Configuration

### Environment Variables

- `PERSONAL_TOKEN`: GitHub personal access token for deployment
- `NODE_ENV`: Node.js environment (development/production)

### Build Configuration

- **Python Version**: 3.11+
- **Node.js Version**: 18+
- **MkDocs Version**: 1.5.0+
- **React Version**: 18.0+

## 🐛 Debugging

### Common Issues

1. **Build Failures**:
   - Check Python and Node.js versions
   - Verify all dependencies are installed
   - Check for syntax errors in templates

2. **CSS Issues**:
   - Verify PostCSS configuration
   - Check for CSS syntax errors
   - Ensure Tailwind CSS is loaded

3. **JavaScript Issues**:
   - Check browser console for errors
   - Verify React is loaded correctly
   - Check for JavaScript syntax errors

### Debug Commands

```bash
# Debug MkDocs build
mkdocs build --verbose

# Debug CSS processing
npx postcss custom_theme/assets/css/custom.css --verbose

# Debug JavaScript
node -c custom_theme/assets/js/custom.js
```

## 📚 Resources

### Documentation

- [MkDocs Documentation](https://www.mkdocs.org/)
- [Tailwind CSS Documentation](https://tailwindcss.com/docs)
- [React Documentation](https://reactjs.org/docs)
- [PostCSS Documentation](https://postcss.org/)

### Tools

- [HTML Validator](https://validator.w3.org/)
- [CSS Validator](https://jigsaw.w3.org/css-validator/)
- [Lighthouse](https://developers.google.com/web/tools/lighthouse)
- [Accessibility Testing](https://www.w3.org/WAI/ER/tools/)

## 🤝 Contributing

### Development Guidelines

1. **Code Style**: Follow existing code patterns
2. **Testing**: Add tests for new features
3. **Documentation**: Update documentation for changes
4. **Performance**: Ensure changes don't impact performance

### Pull Request Process

1. Fork the repository
2. Create a feature branch
3. Make your changes
4. Add tests if applicable
5. Update documentation
6. Submit a pull request

## 📞 Support

If you encounter issues during development:

1. Check the [Issues](https://github.com/asap-programmer/static-site-python/issues) page
2. Create a new issue with detailed information
3. Contact the maintainer via GitHub

---

**Happy coding! 🚀**
