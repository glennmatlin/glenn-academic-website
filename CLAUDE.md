# CLAUDE.md - Assistant Guidelines

## Build Commands
- Local development: `hugo server`
- Build production: `hugo --gc --minify`
- Preview build: `hugo --gc --minify --buildFuture`
- Production build: `hugo --gc --minify -b $URL`
- Preview deploy: `hugo --gc --minify --buildFuture -b $DEPLOY_PRIME_URL`
- Branch deploy: `hugo --gc --minify -b $DEPLOY_PRIME_URL`
- Check site: `hugo server` then visit http://localhost:1313/

## Git Workflow
- Create feature branch: `git checkout -b branch-name`
- Add changes: `git add .`
- Commit changes: `git commit -m "Description of changes"`
- Push to remote: `git push -u origin branch-name`
- Site has enableGitInfo: true in config.yaml

## Website Organization
- Academic portfolio site built with Hugo using Wowchemy Academic theme
- Primary sections: About, Experience, Publications, Projects, Posts
- Knowledge base areas: Python, Cheatsheets
- Personal info in `content/authors/glenn/`
- Widget-based home page in `content/home/`
- Company logos in `assets/media/icons/brands/` directory

## Content Guidelines
- Use YAML frontmatter for all content files
- Widget structure defined in `widget:` parameter
- Home page sections in `content/home/*.md`
- Blog posts in `content/post/*/index.md` with featured images
- Resume/CV stored in `static/uploads/resume.pdf` and `cv.pdf`
- Place images in `assets/media/` directories
- Project pages in `content/project/*/index.md` with featured images
- Publications in `content/publication/*/index.md`

## Widget Structure
- Each widget defines a section on the home page
- Common widget params: title, subtitle, weight, active
- Experience widget uses company logos from assets/media/icons/brands/
- Skills widget supports detailed descriptions for each skill
- Projects widget supports filtering by category
- Use weight parameter to control section order (lower = higher on page)

## File Structure Patterns
- Section pages use `_index.md`
- Individual pages use `index.md` in subdirectories with their resources
- Content organization:
  - `/content/`: All website content organized by section
  - `/assets/`: Media and other static resources
  - `/config/_default/`: Hugo configuration files
  - `/static/uploads/`: PDFs and other downloadable files

## Configuration Files
- Main config: `/config/_default/config.yaml`
- Menus config: `/config/_default/menus.yaml`
- Parameters: `/config/_default/params.yaml`
- Languages: `/config/_default/languages.yaml`
- Navigation links and sections enabled in menus.yaml