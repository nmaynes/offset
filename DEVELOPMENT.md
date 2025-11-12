# Offset Theme Development Guide

## Overview

The offset theme is built for **Hugo v0.146.0+** using the new template system introduced in that version.

## Template Structure (New System)

```
themes/offset/
├── layouts/
│   ├── baseof.html           # Base template (not in _default/)
│   ├── home.html             # Homepage template
│   ├── single.html           # Single post/page template
│   ├── list.html             # List/section template
│   ├── page.html             # Custom page template
│   ├── section.html          # Section template
│   ├── taxonomy.html         # Taxonomy template
│   ├── _partials/            # Partials (note the underscore)
│   │   ├── pagination.html
│   │   └── list.html
│   └── _shortcodes/          # Shortcodes (note the underscore)
├── static/
│   └── css/
│       └── style.css         # Main stylesheet
└── theme.toml
```

### Key Differences from Old Hugo

**Hugo v0.146.0+ Changes:**
- ✅ Templates in `layouts/` root (no `_default/` folder)
- ✅ `_partials/` not `partials/`
- ✅ `_shortcodes/` not `shortcodes/`
- ✅ `home.html` not `index.html`
- ✅ Simplified lookup order

## Testing Locally

### Install Hugo (Latest Version)

```bash
# On Ubuntu/WSL
sudo snap install hugo

# On Windows (with Chocolatey)
choco install hugo-extended

# On macOS
brew install hugo

# Verify version (should be 0.146.0 or higher)
hugo version
```

### Build and Serve

```bash
# Serve with live reload (including drafts)
hugo server -D

# Serve on specific port
hugo server -D --port 1313

# Build to public/ folder
hugo

# Build with minification (production)
hugo --minify
```

Visit `http://localhost:1313` in your browser.

## Customizing Styles

All styles are in `static/css/style.css`.

### CSS Variables

Change these variables to customize the theme's appearance:

```css
:root {
    --primary-color: #2c3e50;     /* Main heading color */
    --secondary-color: #3498db;   /* Accent color */
    --text-color: #333;           /* Body text */
    --bg-color: #fff;             /* Background */
    --border-color: #e0e0e0;      /* Borders */
    --link-color: #3498db;        /* Links */
    --link-hover: #2980b9;        /* Link hover state */
    --max-width: 800px;           /* Content width */
    --spacing: 1.5rem;            /* Base spacing */
}
```

## Template Files

### baseof.html
Base template that wraps all pages. Contains:
- HTML document structure
- Header with site title and navigation
- Main content area (filled by child templates)
- Footer with copyright and social links
- Link to stylesheet

### home.html
Homepage template that:
- Lists recent posts from `mainSections`
- Shows post titles, dates, categories, and summaries
- Includes pagination

### single.html
Individual post/page template that:
- Displays post title, date, categories, and tags
- Renders post content
- Shows previous/next navigation

### list.html
Section and taxonomy listing template that:
- Shows section title and description
- Lists all pages in the section
- Includes pagination

## Template Blocks

The base template (`baseof.html`) defines these blocks:

- `{{ block "title" . }}` - Page title
- `{{ block "styles" . }}` - Additional CSS (for page-specific styles)
- `{{ block "header" . }}` - Site header
- `{{ block "main" . }}` - Main content (required in child templates)
- `{{ block "footer" . }}` - Site footer

### Using Blocks in Child Templates

```html
{{ define "main" }}
<main>
    <!-- Your content here -->
</main>
{{ end }}
```

## Partials

Partials are reusable template components in `layouts/_partials/`.

### Using Partials

```html
{{ partial "pagination.html" . }}
```

### Available Partials

- `pagination.html` - Previous/Next page navigation
- `list.html` - Generic list template

## Creating New Templates

### Custom Single Page Layout

Create `layouts/contact.html`:
```html
{{ define "main" }}
<main>
    <h1>{{ .Title }}</h1>
    <div class="content">
        {{ .Content }}
    </div>
    <!-- Add custom contact form here -->
</main>
{{ end }}
```

Use in front matter:
```yaml
layout: contact
```

### Custom Section Template

Create `layouts/blog/list.html` for a custom blog section layout.

## Hugo's Template Lookup Order (v0.146.0+)

Hugo looks for templates in this order:

1. Custom layout from front matter
2. Page kind (home, single, list, etc.)
3. Output format (html, rss, etc.)
4. Language code
5. Path-based matching

**Example:** For a blog post at `content/post/my-article.md`:
1. `layouts/post/single.html`
2. `layouts/single.html`
3. `layouts/baseof.html` (wraps the selected template)

## CSS Classes Reference

### Layout Classes
- `.post-list` - List of posts
- `.post-title` - Post title
- `.post-date` - Post publication date
- `.post-meta` - Post metadata (categories, tags)

### Article Classes
- `.content` - Main post content
- `.tags` - Tag list
- `.post-navigation` - Prev/Next links
- `.prev-post` - Previous post link
- `.next-post` - Next post link

### Utility Classes
- `.pagination` - Pagination controls
- `.disabled` - Disabled state

## Development Workflow

1. **Make changes** to templates or CSS
2. **Hugo server auto-reloads** (if running `hugo server -D`)
3. **Check browser** to verify changes
4. **Commit** when satisfied
5. **Push to main** to trigger GitHub Actions deployment

## Common Tasks

### Adding a New Partial

1. Create file in `layouts/_partials/your-partial.html`
2. Use with `{{ partial "your-partial.html" . }}`

### Adding Custom CSS

Add to `static/css/style.css` or create new CSS file and link in `baseof.html`:

```html
{{ block "styles" . }}
<link rel="stylesheet" href="{{ "css/custom.css" | relURL }}">
{{ end }}
```

### Creating a Custom Shortcode

1. Create file in `layouts/_shortcodes/your-shortcode.html`
2. Use in content: `{{< your-shortcode >}}`

## Troubleshooting

**Issue**: Templates not loading
- Ensure templates are in `layouts/` root, not `layouts/_default/`
- Check template uses `{{ define "main" }}`

**Issue**: Partials not found
- Verify partials are in `layouts/_partials/` (with underscore)

**Issue**: CSS not loading
- Check `static/css/style.css` exists
- Verify baseof.html has `<link>` tag with correct path

**Issue**: Homepage not showing posts
- Check `config.yaml` has `mainSections` parameter
- Verify content is in specified sections (e.g., `content/post/`)
- Ensure posts aren't marked `draft: true`

## Hugo Documentation

- [Template Lookup Order](https://gohugo.io/templates/lookup-order/)
- [New Template System](https://gohugo.io/templates/new-templatesystem-overview/)
- [Base Templates](https://gohugo.io/templates/base/)
