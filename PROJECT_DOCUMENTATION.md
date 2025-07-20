# Glenn Matlin Website - Project Documentation

## Overview

This is a Quarto-based personal/academic website for Glenn Matlin, featuring blog posts, research projects, academic publications, and a technical knowledge base. The site is built using Quarto, a modern scientific publishing system, and is deployed via GitHub Pages.

**Live Site**: [https://glennmatlin.com](https://glennmatlin.com)

## Table of Contents

1. [Project Structure](#project-structure)
2. [Technology Stack](#technology-stack)
3. [Content Management](#content-management)
4. [Development Workflow](#development-workflow)
5. [Deployment](#deployment)
6. [Configuration](#configuration)
7. [Styling and Customization](#styling-and-customization)
8. [Maintenance](#maintenance)

## Project Structure

```
website/
├── _quarto.yml              # Main Quarto configuration
├── _site/                   # Generated static site (do not edit)
├── index.qmd                # Homepage
├── about.qmd                # About page
├── blog.qmd                 # Blog listing page
├── projects.qmd             # Projects listing page
├── publications.qmd         # Publications listing page
├── 404.qmd                  # Custom 404 page
├── styles.css               # Custom CSS styles
│
├── post/                    # Blog posts
│   └── [post-name]/
│       ├── index.md        # Post content
│       └── featured.png    # Post thumbnail
│
├── project/                 # Research projects
│   └── [project-name]/
│       ├── index.md        # Project details
│       └── featured.png    # Project image
│
├── publication/             # Academic publications
│   └── [pub-name]/
│       ├── index.md        # Publication details
│       ├── cite.bib        # Citation
│       └── [pub-name].pdf  # PDF file
│
├── knowledgebase/          # Technical documentation
│   ├── cheatsheets/        # Quick reference guides
│   └── python/             # Python-specific docs
│
├── assets/                 # Static assets
│   └── media/             # Images and icons
│
├── uploads/               # Downloadable files
│   └── resume.pdf        # CV/Resume
│
└── docs/                  # Internal documentation
    ├── GLENN.md          # Professional profile
    └── IMPROVEMENT_PLAN.md # Site improvement roadmap
```

## Technology Stack

- **Static Site Generator**: Quarto 1.7.32
- **Theme**: Cosmo (Bootstrap-based)
- **Languages**: Markdown, YAML, HTML, CSS
- **Python Environment**: UV package manager
- **Deployment**: GitHub Pages
- **Version Control**: Git

## Content Management

### Adding a New Blog Post

1. Create a new directory in `post/`:
   ```bash
   mkdir post/my-new-post
   ```

2. Create `index.md` with the following frontmatter:
   ```yaml
   ---
   title: "Your Post Title"
   description: "Brief description of your post"
   author: "Glenn Matlin"
   date: "2024-01-20"
   categories: [AI, Machine Learning, Research]
   image: "featured.png"
   draft: false
   ---
   ```

3. Add your content in Markdown format below the frontmatter

4. Add a featured image named `featured.png` (recommended: 1200x630px)

### Adding a New Project

1. Create a new directory in `project/`:
   ```bash
   mkdir project/my-project
   ```

2. Create `index.md` with project details:
   ```yaml
   ---
   title: "Project Title"
   description: "Project description"
   date: 2024-01-20
   categories: [Research, AI, NLP]
   image: "featured.png"
   links:
     - icon: fa fa-github
       name: Code
       url: https://github.com/username/project
   ---
   ```

### Adding a New Publication

1. Create a new directory in `publication/`:
   ```bash
   mkdir publication/paper-name
   ```

2. Create `index.md` with publication metadata:
   ```yaml
   ---
   title: "Paper Title"
   authors: ["Glenn Matlin", "Co-Author Name"]
   date: 2024-01-20
   publication: "Conference/Journal Name"
   publication_types: ["2"]  # 2 = Journal article
   abstract: "Paper abstract..."
   featured: false
   doi: "10.1234/example"
   links:
     - name: PDF
       url: paper-name.pdf
   ---
   ```

3. Add `cite.bib` with BibTeX citation
4. Add the PDF file if available

### Adding Knowledge Base Articles

1. Create markdown files in appropriate subdirectory:
   ```bash
   knowledgebase/category/article-name.md
   ```

2. Use standard markdown with optional frontmatter:
   ```yaml
   ---
   title: "Article Title"
   description: "Brief description"
   date: 2024-01-20
   ---
   ```

## Development Workflow

### Initial Setup

1. Clone the repository:
   ```bash
   git clone [repository-url]
   cd website
   ```

2. Install UV (Python package manager):
   ```bash
   curl -LsSf https://astral.sh/uv/install.sh | sh
   ```

3. Install dependencies:
   ```bash
   uv sync
   ```

### Local Development

1. Start the development server:
   ```bash
   quarto preview
   ```
   This will open the site at `http://localhost:4000` with live reload

2. Make changes to content files (`.qmd`, `.md`)

3. Preview changes in the browser (auto-reloads)

### Building the Site

Generate the static site:
```bash
quarto render
```

This creates/updates the `_site/` directory with the static HTML files.

## Deployment

The site is deployed automatically via GitHub Pages when changes are pushed to the main branch.

### Manual Deployment

1. Build the site:
   ```bash
   quarto render
   ```

2. Publish to GitHub Pages:
   ```bash
   quarto publish gh-pages
   ```

### Custom Domain

The custom domain is configured via:
- `CNAME` file in the repository root
- DNS settings pointing to GitHub Pages

## Configuration

### Main Configuration (_quarto.yml)

Key settings:
```yaml
project:
  type: website
  output-dir: _site

website:
  title: "Glenn Matlin"
  site-url: "https://glennmatlin.com"
  description: "Personal website of Glenn Matlin"
  navbar:
    # Navigation structure
  page-footer:
    # Footer content

format:
  html:
    theme: cosmo
    css: styles.css
```

### Python Dependencies (pyproject.toml)

Managed by UV:
```toml
dependencies = [
    "jupyter>=1.0.0",
    "pyyaml>=6.0.1",
    "plotly>=5.0.0",
    "pandas>=2.0.0"
]
```

## Styling and Customization

### Custom CSS (styles.css)

Key style sections:
- **Global Typography**: Font settings and color scheme
- **Hero Banner**: Homepage gradient banner
- **Feature Cards**: Project/post cards with hover effects
- **Navigation**: Navbar and dropdown styling
- **Responsive Design**: Mobile-friendly layouts

### Color Palette

- Primary: `#667eea` (Purple)
- Secondary: `#764ba2` (Dark Purple)
- Text: `#2c3e50` (Dark Gray)
- Background: `#f8f9fa` (Light Gray)

### Modifying Styles

1. Edit `styles.css` for global changes
2. Use inline styles in markdown for specific elements
3. Add new CSS classes as needed

## Maintenance

### Regular Tasks

1. **Content Updates**:
   - Add new blog posts monthly
   - Update projects with progress
   - Add new publications as published

2. **Technical Maintenance**:
   - Update Quarto version quarterly
   - Review and update dependencies
   - Check for broken links

3. **Performance**:
   - Optimize images (max 1MB, prefer WebP)
   - Monitor page load times
   - Review Google Analytics (if configured)

### Backup Strategy

1. Git repository serves as primary backup
2. Regular exports of site content
3. Backup of any external assets

### Troubleshooting

**Common Issues**:

1. **Build Failures**:
   - Check YAML syntax in frontmatter
   - Verify image paths are correct
   - Ensure all dependencies are installed

2. **Styling Issues**:
   - Clear browser cache
   - Check for CSS conflicts
   - Verify responsive breakpoints

3. **Deployment Issues**:
   - Ensure GitHub Pages is enabled
   - Check CNAME configuration
   - Verify build succeeds locally

### SEO Optimization

1. **Page Metadata**:
   - Add descriptions to all pages
   - Use meaningful titles
   - Include Open Graph tags

2. **Performance**:
   - Optimize images
   - Minimize CSS/JS
   - Enable caching

3. **Content**:
   - Use semantic HTML
   - Include alt text for images
   - Maintain consistent URL structure

## Additional Resources

- [Quarto Documentation](https://quarto.org/)
- [GitHub Pages Guide](https://pages.github.com/)
- [UV Package Manager](https://github.com/astral-sh/uv)
- [Bootstrap Documentation](https://getbootstrap.com/docs/)

## Version History

- **2025-01-20**: Migrated from Hugo to Quarto
- **2025-01-20**: Added custom styling and hero banner
- **2025-01-20**: Fixed frontend aesthetics and layout issues

---

For questions or issues, contact: glenn@glennmatlin.doctor