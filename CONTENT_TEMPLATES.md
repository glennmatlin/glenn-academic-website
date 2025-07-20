# Content Templates - Glenn Matlin Website

This guide provides templates for creating new content on the website. Copy and modify these templates as needed.

## Blog Post Template

Create a new directory: `post/your-post-name/`

### `post/your-post-name/index.md`

```markdown
---
title: "Your Blog Post Title"
description: "A brief description of your post (150-160 characters for SEO)"
author: "Glenn Matlin"
date: 2024-01-20
categories: [Machine Learning, AI, Research]
tags: [llm, evaluation, nlp]
image: "featured.png"
image-alt: "Description of the featured image"
draft: false
toc: true
number-sections: true
highlight-style: github
---

## Introduction

Start with a compelling introduction that explains what the post is about and why it matters.

## Main Content

### Section 1

Your content here. Use markdown formatting:

- **Bold text** for emphasis
- *Italic text* for subtle emphasis
- `code snippets` for technical terms

### Code Examples

```python
# Python code example
def hello_world():
    print("Hello, World!")
```

### Images

![Image description](image.png)

### Mathematical Formulas

For inline math: $x = \frac{-b \pm \sqrt{b^2 - 4ac}}{2a}$

For display math:
$$
E = mc^2
$$

## Key Takeaways

- Summarize main points
- Provide actionable insights
- Link to related resources

## References

1. Author, A. (2024). *Title of Reference*. Publisher.
2. [Link to Resource](https://example.com)

## Further Reading

- [Related Blog Post](../another-post/)
- [Project Page](../../project/related-project/)
```

### Featured Image Requirements
- **Filename**: `featured.png`
- **Dimensions**: 1200x630px (optimal for social media)
- **Format**: PNG or JPG
- **File size**: < 500KB

## Project Template

Create a new directory: `project/your-project-name/`

### `project/your-project-name/index.md`

```markdown
---
title: "Project Title"
description: "Brief project description for preview cards"
author: "Glenn Matlin"
date: 2024-01-20
categories: [Machine Learning, NLP, Computer Vision]
tags: [pytorch, transformers, research]
image: "featured.png"
image-alt: "Project visualization or logo"
links:
  - icon: fab fa-github
    name: Code
    url: https://github.com/gmatlin/project-name
  - icon: fas fa-file-pdf
    name: Paper
    url: https://arxiv.org/abs/xxxx.xxxxx
  - icon: fas fa-globe
    name: Demo
    url: https://demo.example.com
---

## Overview

Brief overview of the project, its goals, and significance.

## Motivation

Why this project exists and what problem it solves.

## Technical Approach

### Architecture

Describe the technical architecture, frameworks used, and design decisions.

### Key Features

- Feature 1: Description
- Feature 2: Description
- Feature 3: Description

## Results

### Performance Metrics

| Metric | Value | Baseline |
|--------|-------|----------|
| Accuracy | 95.2% | 92.1% |
| F1 Score | 0.94 | 0.89 |
| Speed | 100ms | 250ms |

### Visualizations

Include charts, graphs, or screenshots demonstrating results.

## Code Example

```python
from my_project import Model

# Initialize model
model = Model(config='large')

# Run inference
results = model.predict(data)
```

## Installation

```bash
pip install project-name
```

## Future Work

- Planned improvements
- Research directions
- Community contributions welcome

## Citation

```bibtex
@article{matlin2024project,
  title={Project Title},
  author={Matlin, Glenn},
  journal={Conference/Journal},
  year={2024}
}
```

## Acknowledgments

Thanks to collaborators, funding sources, etc.
```

## Publication Template

Create a new directory: `publication/paper-name-year/`

### `publication/paper-name-year/index.md`

```markdown
---
title: "Full Paper Title as Published"
authors:
  - Glenn Matlin
  - Co-Author Name
  - Another Co-Author
date: 2024-01-20
publishDate: 2024-01-20
publication: "Conference or Journal Name"
publication_short: "CONF 2024"
publication_types: ["1"]  # 0: Uncategorized, 1: Conference paper, 2: Journal article
abstract: "Copy the official abstract from your paper here. This should be the complete abstract as published, providing a comprehensive summary of the research, methodology, results, and conclusions."
featured: true
doi: "10.1109/CONFERENCE.2024.1234567"
url_pdf: "paper-name.pdf"
url_code: "https://github.com/gmatlin/paper-code"
url_dataset: "https://github.com/gmatlin/paper-data"
url_poster: "poster.pdf"
url_project: "../../project/related-project/"
url_slides: "slides.pdf"
url_source: ""
url_video: "https://youtube.com/watch?v=xxxxx"
image:
  filename: "featured.png"
  focal_point: "Center"
  preview_only: false
tags:
  - Machine Learning
  - Computer Vision
  - Deep Learning
---

## Overview

A brief, accessible summary of the paper for a general audience.

## Key Contributions

1. **Contribution 1**: Brief description
2. **Contribution 2**: Brief description
3. **Contribution 3**: Brief description

## Resources

- [Paper PDF](paper-name.pdf)
- [Code Repository](https://github.com/gmatlin/paper-code)
- [Presentation Slides](slides.pdf)
- [Conference Talk](https://youtube.com/watch?v=xxxxx)

## Citation

```bibtex
@inproceedings{matlin2024papername,
  title={Full Paper Title as Published},
  author={Matlin, Glenn and Co-Author, Name and Another, Author},
  booktitle={Proceedings of the Conference Name},
  pages={123--456},
  year={2024},
  organization={Publisher}
}
```

## Related Work

- [Related Project](../../project/project-name/)
- [Follow-up Paper](../next-paper-2024/)
```

### Required Files

1. **Paper PDF**: `paper-name.pdf`
2. **BibTeX Citation**: `cite.bib`
3. **Featured Image**: `featured.png` (1200x630px)
4. **Optional**: `poster.pdf`, `slides.pdf`

### Publication Types
- 0: Uncategorized
- 1: Conference paper
- 2: Journal article
- 3: Preprint / Working Paper
- 4: Report
- 5: Book
- 6: Book section
- 7: Thesis
- 8: Patent

## Knowledge Base Article Template

Create file: `knowledgebase/category/article-name.md`

```markdown
---
title: "Article Title"
description: "Brief description for search and preview"
date: 2024-01-20
lastmod: 2024-01-20
toc: true
---

# Article Title

## Overview

Brief introduction to the topic.

## Prerequisites

- Required knowledge
- Software requirements
- System requirements

## Main Content

### Step 1: Getting Started

Detailed instructions with examples.

### Step 2: Configuration

```yaml
# Configuration example
setting1: value1
setting2: value2
```

### Step 3: Advanced Usage

More complex examples and edge cases.

## Common Issues

### Issue 1: Error Message

**Problem**: Description of the issue

**Solution**: Step-by-step fix

### Issue 2: Another Problem

**Problem**: Description

**Solution**: How to resolve

## Quick Reference

| Command | Description | Example |
|---------|-------------|---------|
| `cmd1` | What it does | `cmd1 --flag` |
| `cmd2` | What it does | `cmd2 arg` |

## Best Practices

1. Recommendation 1
2. Recommendation 2
3. Recommendation 3

## Additional Resources

- [Official Documentation](https://example.com)
- [Related Tutorial](../another-article/)
- [Video Guide](https://youtube.com)

## Conclusion

Summary and next steps.
```

## Image Guidelines

### Featured Images
- **Blog Posts**: 1200x630px (16:9 ratio)
- **Projects**: 1200x630px or 800x600px
- **Publications**: 1200x630px

### Optimization
- Use PNG for graphics with transparency
- Use JPG for photographs
- Compress images (target < 500KB)
- Use descriptive filenames
- Always include alt text

### Tools
- [TinyPNG](https://tinypng.com/) - Image compression
- [Canva](https://canva.com/) - Create featured images
- [Unsplash](https://unsplash.com/) - Free stock photos

## SEO Best Practices

1. **Title**: 50-60 characters
2. **Description**: 150-160 characters
3. **URL**: Use descriptive slugs (e.g., `llm-evaluation-challenges`)
4. **Headers**: Use proper hierarchy (H1 → H2 → H3)
5. **Images**: Always include alt text
6. **Links**: Use descriptive anchor text

## Writing Tips

1. **Start Strong**: Hook readers in the first paragraph
2. **Use Subheadings**: Break content into scannable sections
3. **Include Examples**: Code snippets, images, or demos
4. **Add Value**: Provide insights not found elsewhere
5. **Call to Action**: End with next steps or related resources

---

*Last updated: January 20, 2025*