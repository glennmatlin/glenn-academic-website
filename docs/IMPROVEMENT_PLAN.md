# Website Improvement Plan

This document outlines a plan for improving the Quarto-based academic website. The improvements are broken down into three categories.

---

### Part 1: Configuration, SEO, and Robustness

These are foundational changes to make the site work better and be more discoverable.

1.  **Add a Custom 404 Page:** Create a custom `404.qmd` page that matches the site's branding and helps users who follow a broken link.
2.  **Enhance the Footer:** Add a copyright notice (e.g., "© 2025 Glenn Matlin") and a "Published with Quarto" notice to `_quarto.yml` for a more professional touch.
3.  **Fix Analytics Placeholder:** Remove the placeholder Google Analytics ID (`G-XXXXXXXXXX`) from `_quarto.yml` to prevent errors.
4.  **Improve Page Metadata:** Add a `description` field to the YAML frontmatter of key pages (`index.qmd`, `about.qmd`, `research.qmd`, etc.) to improve search engine optimization (SEO).

### Part 2: Content Enrichment and Consistency

This focuses on making existing content more detailed and uniform.

1.  **Enrich Publications Data:** Add valuable fields like `abstract`, `doi` (link to the paper), and `pdf` (link to a PDF file) to each entry in `publications/papers.yml`.
2.  **Standardize Project Pages:** Analyze project pages in `/projects/` to ensure a consistent structure, including a featured image (`image:`), tags (`tags:`), and links to the code repository (`repo_url:`) and a live demo (`demo_url:`).
3.  **Refine the CV Page:** Examine `cv.qmd` and suggest either embedding the PDF directly on the page or converting the CV content into HTML for better accessibility and searchability.

### Part 3: User Experience and Polish

These are smaller details that create a more polished experience.

1.  **Ensure Image Accessibility:** Check main pages (`index.qmd`, `about.qmd`) to ensure that all images have descriptive `alt` text for accessibility and SEO.
2.  **Add a Full Set of Favicons:** Add a comprehensive set of favicons for different devices (Apple touch icons, Android icons, etc.) and a `site.webmanifest` file.
