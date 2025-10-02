# Welcome to My Docs

**A modern static site built with MkDocs and custom React theme**

This project demonstrates the power of combining MkDocs with modern frontend technologies like React and Tailwind CSS to create beautiful, responsive documentation sites.

## 🚀 Features

- **Modern Design**: Built with Tailwind CSS for a clean, professional look
- **React Integration**: Interactive components powered by React
- **Responsive Layout**: Works perfectly on all devices
- **Fast Performance**: Optimized with PostCSS and minification
- **SEO Optimized**: Proper meta tags and semantic HTML
- **Accessibility**: WCAG compliant design patterns

## 🛠️ Technology Stack

- **MkDocs**: Static site generator
- **React**: Interactive UI components
- **Tailwind CSS**: Utility-first CSS framework
- **PostCSS**: CSS processing and optimization
- **GitHub Actions**: CI/CD pipeline
- **GitHub Pages**: Hosting platform

## 📖 Getting Started

### Prerequisites

- Python 3.11+
- Node.js 18+
- Git

### Installation

1. Clone the repository:
   git clone https://github.com/asap-programmer/static-site-python.git
   cd static-site-python

2. Install Python dependencies:
   pip install -r requirements.txt

3. Install Node.js dependencies:

```bash
npm install
```

### Development

Start the development server:

```bash
mkdocs serve
```

Build the site:

```bash
mkdocs build
```

Process CSS with PostCSS:

```bash
npm run build:css
```

## 🎨 Customization

The theme is fully customizable through:

- **HTML Templates**: Located in `custom_theme/`
- **CSS Styles**: Custom styles in `custom_theme/assets/css/`
- **JavaScript**: Interactive features in `custom_theme/assets/js/`
- **Configuration**: Site settings in `mkdocs.yml`

## 📁 Project Structure

```
static-site-python/
├── custom_theme/          # Custom theme files
│   ├── base.html         # Base template
│   ├── main.html         # Main page template
│   └── assets/           # Static assets
│       ├── css/          # Custom CSS
│       └── js/           # Custom JavaScript
├── docs/                 # Documentation content
│   └── index.md         # Homepage content
├── .github/workflows/    # GitHub Actions
├── mkdocs.yml           # MkDocs configuration
├── requirements.txt     # Python dependencies
├── package.json         # Node.js dependencies
└── postcss.config.js    # PostCSS configuration
```

## 🔧 Build Process

The site uses a sophisticated build pipeline:

1. **HTML Validation**: Ensures all HTML is valid
2. **CSS Processing**: PostCSS with autoprefixer and minification
3. **JavaScript Optimization**: Minification and bundling
4. **Static Generation**: MkDocs builds the final site
5. **Deployment**: Automatic deployment to GitHub Pages

## 📝 License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

## 👨‍💻 Author

**Ilvir Nizaev** - _Initial work_ - [GitHub](https://github.com/asap-programmer)

## 🙏 Acknowledgments

- MkDocs team for the excellent static site generator
- Tailwind CSS for the utility-first CSS framework
- React team for the powerful UI library
- GitHub for providing free hosting with GitHub Pages
