# Glenn Matlin's Personal Website

[![Quarto](https://img.shields.io/badge/Built%20with-Quarto-blue)](https://quarto.org/)
[![License](https://img.shields.io/badge/License-MIT-green)](LICENSE.md)
[![Website](https://img.shields.io/badge/Website-glennmatlin.com-brightgreen)](https://glennmatlin.com)

Personal academic website featuring blog posts, research projects, publications, and technical documentation.

## 🚀 Quick Start

### Prerequisites

- [Quarto](https://quarto.org/docs/get-started/) (v1.7+)
- Python 3.12+
- [UV](https://github.com/astral-sh/uv) package manager

### Setup

1. **Clone the repository**
   ```bash
   git clone https://github.com/gmatlin/website.git
   cd website
   ```

2. **Install UV** (if not already installed)
   ```bash
   curl -LsSf https://astral.sh/uv/install.sh | sh
   ```

3. **Install dependencies**
   ```bash
   uv sync
   ```

4. **Start development server**
   ```bash
   quarto preview
   ```
   
   Your site will be available at `http://localhost:4000`

## 📁 Project Structure

```
website/
├── _quarto.yml          # Main configuration
├── index.qmd            # Homepage
├── about.qmd            # About page
├── blog.qmd             # Blog listing
├── projects.qmd         # Projects listing
├── publications.qmd     # Publications listing
├── post/               # Blog posts
├── project/            # Project pages
├── publication/        # Publication entries
├── knowledgebase/      # Technical docs
├── assets/             # Images and media
└── styles.css          # Custom styling
```

## ✍️ Adding Content

### New Blog Post

```bash
# Create post directory
mkdir post/my-new-post

# Create post content
cat > post/my-new-post/index.md << 'EOF'
---
title: "My New Post"
date: 2024-01-20
categories: [AI, Research]
image: featured.png
---

Your post content here...
EOF
```

### New Project

```bash
# Create project directory
mkdir project/my-project

# Add project details
# See project/flame/index.md for template
```

### New Publication

```bash
# Create publication directory
mkdir publication/paper-2024

# Add publication metadata
# See publication/example/index.md for template
```

## 🛠️ Development

### Build the site
```bash
quarto render
```

### Preview with live reload
```bash
quarto preview
```

### Check for issues
```bash
quarto check
```

## 🚀 Deployment

The site automatically deploys to GitHub Pages when you push to the `main` branch.

### Manual deployment
```bash
quarto publish gh-pages
```

## 🎨 Customization

- **Styles**: Edit `styles.css` for custom styling
- **Navigation**: Modify `_quarto.yml` for menu structure
- **Theme**: Change theme in `_quarto.yml` (currently using "cosmo")

## 📚 Documentation

- [Full Project Documentation](PROJECT_DOCUMENTATION.md) - Comprehensive guide
- [Improvement Plan](docs/IMPROVEMENT_PLAN.md) - Roadmap for enhancements
- [Quarto Docs](https://quarto.org/docs/websites/) - Official Quarto documentation

## 🐛 Troubleshooting

### Common Issues

1. **Preview not working**: Ensure Quarto is installed and in PATH
2. **Build errors**: Check YAML frontmatter syntax
3. **Missing dependencies**: Run `uv sync` to install

### Useful Commands

```bash
# Clear cache and rebuild
rm -rf _site .quarto
quarto render

# Check Quarto installation
quarto check

# Update Quarto
# Visit https://quarto.org/docs/get-started/
```

## 📄 License

This project is licensed under the MIT License - see [LICENSE.md](LICENSE.md) for details.

## 📧 Contact

Glenn Matlin - glenn@glennmatlin.doctor

---

Built with ❤️ using [Quarto](https://quarto.org/)