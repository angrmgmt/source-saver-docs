# GitHub Wiki Documentation Setup

This directory contains markdown files ready to be uploaded to your GitHub repository's Wiki.

## How to Set Up GitHub Wiki

### Option 1: Upload via GitHub Web Interface

1. Go to your GitHub repository
2. Click on the "Wiki" tab
3. Click "Create the first page" or "New Page"
4. Copy and paste the content from each `.md` file in this directory
5. Use the filename (without `.md`) as the page title

### Option 2: Clone Wiki Repository

GitHub Wikis are actually Git repositories themselves:

```bash
# Clone the wiki repository
git clone https://github.com/your-username/source-saver.wiki.git

# Copy all markdown files
cp docs/wiki/*.md source-saver.wiki/

# Commit and push
cd source-saver.wiki
git add .
git commit -m "Add comprehensive API documentation"
git push origin master
```

## File Structure

The documentation is organized as follows:

- **Home.md** - Main landing page with overview and quick start
- **OBS-API.md** - OBS Studio integration endpoints
- **Sources-API.md** - Browser source management endpoints
- **Scenes-API.md** - Scene and source positioning endpoints
- **System-API.md** - Health, monitoring, and system endpoints
- **WebSocket-API.md** - Real-time WebSocket communication

## Navigation

The documentation includes cross-references between pages using GitHub Wiki's link syntax:

- `[Link Text](Page-Name)` - Links to other wiki pages
- `[Link Text](Page-Name#section)` - Links to specific sections

## Features

✅ **Complete API Coverage** - All REST endpoints documented\
✅ **WebSocket Documentation** - Real-time communication guide\
✅ **Request/Response Examples** - JSON examples for all endpoints\
✅ **Error Handling** - Common error responses and status codes\
✅ **Best Practices** - Usage recommendations and patterns\
✅ **Cross-References** - Easy navigation between related topics

## Maintenance

To update the documentation:

1. Edit the markdown files in this directory
2. Copy updated files to your wiki repository
3. Commit and push changes

## Alternative: GitHub Pages

If you prefer GitHub Pages over Wiki, you can also use these files with Jekyll or other static site generators by adding
appropriate front matter.

## Benefits of GitHub Wiki

- **Version Control** - Full Git history of documentation changes
- **Collaborative** - Team members can edit documentation
- **Searchable** - GitHub's search includes wiki content
- **Integrated** - Linked directly from your repository
- **Markdown Support** - Rich formatting with code highlighting
- **No Build Process** - Immediate publishing of changes

## Deno-Centric Approach

This documentation approach is perfect for Deno projects because:

- **No Dependencies** - Pure markdown, no swagger-ui or other heavy dependencies
- **Native Git Integration** - Fits perfectly with Deno's philosophy
- **Lightweight** - No build process or additional tooling required
- **Maintainable** - Easy to keep in sync with code changes
- **Accessible** - Available to anyone with repository access

---

Your API documentation is now ready to be published on GitHub Wiki! 🎉
