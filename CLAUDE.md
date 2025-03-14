# CLAUDE.md - Assistant Guidelines

## Build Commands
- Local development: `hugo server`
- Build production: `hugo --gc --minify`
- Preview build: `hugo --gc --minify --buildFuture`
- Production build: `hugo --gc --minify -b $URL`
- Preview deploy: `hugo --gc --minify --buildFuture -b $DEPLOY_PRIME_URL`
- Branch deploy: `hugo --gc --minify -b $DEPLOY_PRIME_URL`

## Website Organization
- Academic portfolio site built with Hugo using Wowchemy Academic theme
- Primary sections: About, Experience, Publications, Projects, Posts
- Knowledge base areas: Python, Cheatsheets
- Personal info in `content/authors/glenn/`
- Widget-based home page in `content/home/`

## Content Guidelines
- Use YAML frontmatter for all content files
- Widget structure defined in `widget:` parameter
- Home page sections in `content/home/*.md`
- Blog posts in `content/post/*/index.md` with featured images
- Resume/CV stored in `static/uploads/resume.pdf` and `cv.pdf`
- Place images in `assets/media/` directories

## File Structure Patterns
- Section pages use `_index.md`
- Individual pages use `index.md` in subdirectories with their resources
- Content organization:
  - `/content/`: All website content organized by section
  - `/assets/`: Media and other static resources
  - `/config/_default/`: Hugo configuration files
  - `/static/uploads/`: PDFs and other downloadable files