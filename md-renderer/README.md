# MD Renderer

A lightweight static site generator for rendering markdown files to HTML using 11ty (Eleventy).

## Installation

```bash
npm install
```

## Usage

**Development server with live reload:**
```bash
npm run dev
```

**Build for production:**
```bash
npm run build
```

**Watch for changes:**
```bash
npm run watch
```

## Configuration

- `.eleventy.js` - Main configuration file
- `src/` - Input directory for markdown and templates
- `_site/` - Output directory (generated)

## Directory Structure

```
md-renderer/
├── src/
│   ├── _layouts/        # Page layout templates
│   ├── _includes/       # Reusable components
│   ├── css/             # Stylesheets
│   ├── js/              # JavaScript
│   └── *.md             # Content files
├── _site/               # Built output (gitignored)
├── .eleventy.js         # Configuration
├── package.json         # Dependencies
└── README.md            # This file
```

## Creating Content

Add markdown files in the `src/` directory. Use front matter to define layout and metadata:

```markdown
---
layout: base.njk
title: My Page
---

# My Content

Your markdown here...
```

## Learn More

- [11ty Documentation](https://www.11ty.dev/)
- [Markdown-it](https://github.com/markdown-it/markdown-it)
