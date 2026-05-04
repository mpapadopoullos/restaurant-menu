# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

Static restaurant menu website for YATEL using Alpine.js. No build process—just HTML, CSS, and client-side JavaScript.

## Development

**Preview the site:**
```bash
# Using Python
python3 -m http.server 8000

# Or using PHP
php -S localhost:8000

# Or using node
npx serve . -l 8000
```

Then open http://localhost:8000 in your browser.

## Architecture

**Data-driven design:**
- All menu content lives in `menu.json` (restaurant info, categories, items, tags)
- `index.html` fetches menu.json and renders it using Alpine.js templates
- No hardcoded menu items in HTML

**Theme system:**
- CSS custom properties in `:root` and `[data-theme="dark"]`
- Theme preference persisted in localStorage as `yatel-theme`
- Initial theme resolved on load to prevent flash: checks localStorage, falls back to `prefers-color-scheme`

**Filtering logic:**
- Categories: Show only items from the active category
- Dietary filters: When active, items must have ALL selected tags (AND logic, not OR)
- Both filters work together

**SEO structure:**
- Open Graph, Twitter cards, and JSON-LD structured data embedded in `<head>`
- Restaurant schema with menu sections for search engine discovery
- Static metadata (does not reflect menu.json dynamically)

## Modifying the Menu

**To add/edit menu items:**
Edit `menu.json` directly. Structure:
```json
{
  "restaurant": { "name", "description", "url", "currency", "language" },
  "tags": { "tag-id": { "label", "icon" } },
  "categories": [
    {
      "id": "category-id",
      "name": "Category Name",
      "items": [
        { "name", "description", "price", "tags": ["tag-id"] }
      ]
    }
  ]
}
```

**To add new dietary tags:**
1. Add SVG icon to `icons/` directory
2. Add tag definition to `menu.json` → `tags` object with icon path
3. Tag items by adding the tag ID to their `tags` array

**Updating SEO metadata:**
If restaurant name/description changes, update both:
- `menu.json` → `restaurant` object
- `index.html` → `<head>` meta tags and JSON-LD script

## Design System

- CSS variables in `css/styles.css` define light/dark themes
- Layout uses a centered container (max-width: 720px)
- Desktop (≥640px): 2-column grid for menu items
- Mobile: single column, horizontal scrolling for category/filter chips
- Sticky category navigation bar at top
