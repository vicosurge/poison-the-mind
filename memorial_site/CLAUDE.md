# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

This is a **static HTML memorial website** preserving the creative works and community archives of poisonthemind.com, an online creative community that existed from 2004-2010. Founded by Matthew "Bitterman" Hall.

The site is 100% static with no build system, no backend, and no external dependencies. Files are served directly.

## Repository Structure

```
/                               # Root contains Wayback Machine source archives
├── memorial_site/              # THE ACTUAL WEBSITE (work here)
│   ├── index.html              # Home page
│   ├── gallery.html            # Art gallery (90 images)
│   ├── conversations.html      # Forum posts (548 posts, 58 comments)
│   ├── css/style.css           # Single stylesheet
│   ├── authors/                # Author profile pages (5 authors)
│   ├── stories/                # Story HTML files (20+ stories)
│   ├── archive/                # Timeline, Community, Lost Media pages
│   ├── images/art/             # Full-size gallery images
│   ├── images/thumbs/          # Gallery thumbnails
│   ├── gallery_data.json       # Gallery metadata
│   └── conversations_data.json # Forum posts and comments data
```

## Data Files

**gallery_data.json** - Array of artwork objects:
```json
{"title": "...", "artist": "...", "image": "images/art/...", "thumb": "images/thumbs/...", "tags": "..."}
```

**conversations_data.json** - Contains `forum_posts` and `gallery_comments` arrays:
```json
{
  "forum_posts": [{"post_id": "...", "username": "...", "date": "...", "content": "...", "user_joined": "...", "user_posts": "...", "user_location": "..."}],
  "gallery_comments": [{"comment_id": "...", "username": "...", "content": "...", "item_title": "..."}]
}
```

When regenerating HTML from JSON data, deduplicate by `post_id` or `comment_id` as the source archives contain overlapping captures.

## CSS Theming

The site uses CSS custom properties defined in `css/style.css`:
- `--primary-color: #F16565` (red accent)
- `--bg-dark: #1a1a1a` (dark backgrounds)
- Gallery and conversations pages have additional inline styles for page-specific layouts

## Navigation Pattern

All pages share a consistent navigation structure. When adding/modifying nav links, update all 32 HTML files:
- Root level files use: `href="archive/lost-media.html"`
- Subdirectory files use: `href="../archive/lost-media.html"`

## Adding Content

**New story**: Create HTML file in `stories/`, add to author's page, update `index.html` if featured.

**New gallery image**: Add image to `images/art/` and thumbnail to `images/thumbs/`, add entry to `gallery_data.json`, regenerate `gallery.html`.

**Forum data**: Add to `conversations_data.json`, regenerate `conversations.html` using Python to deduplicate and render all posts.

## Deployment

Serve the `memorial_site/` directory from any static web server or CDN. No build step required.
