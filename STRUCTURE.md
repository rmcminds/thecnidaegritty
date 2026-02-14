# Repository Structure

This document provides a comprehensive overview of the repository structure after integration with the multilingual_journal_club repository.

## Top-Level Directory Structure

```
thecnidaegritty/
├── .github/                      # GitHub-specific files
│   └── workflows/                # GitHub Actions workflows
│       ├── sync_multilingual_content.yml  # Syncs multilingual content
│       └── update_iNatle.yml     # Updates iNatle Shiny app
│
├── .git/                         # Git repository data
├── .gitignore                    # Git ignore patterns
├── .gitmodules                   # Git submodule configuration (if using submodules)
│
├── _config.yml                   # Jekyll site configuration
├── _data/                        # Site data files
│   └── authors.yml               # Author information
│
├── _includes/                    # Reusable HTML components
│   └── social.html               # Social media links
│
├── _layouts/                     # Page layout templates
│   ├── default.html              # Default layout
│   ├── home.html                 # Homepage layout
│   ├── page.html                 # Static page layout
│   └── post.html                 # Blog post layout
│
├── _pages/                       # Static pages
│   ├── about.md                  # About page
│   ├── inatle.md                 # iNatle app page
│   └── rmcminds.md               # Author profile page
│
├── _posts/                       # Standard blog posts
│   └── 2023-07-08-hello-world.md # Example post
│
├── _multilingual_posts/          # Multilingual content (synced from multilingual_journal_club)
│   └── (synced content appears here)
│
├── _multilingual_content/        # Git submodule (optional approach)
│   └── (multilingual_journal_club repository linked here)
│
├── assets/                       # Static assets
│   ├── css/                      # Stylesheets
│   ├── images/                   # Images
│   └── js/                       # JavaScript files
│       └── mastodon_widget.js    # Mastodon timeline widget
│
├── iNatle_raw/                   # Deployed iNatle Shiny app (shinylive export)
│   ├── index.html                # iNatle app entry point
│   ├── shinylive/                # Shinylive runtime
│   └── app.json                  # Shiny app manifest
│
├── photo_annotator/              # Photo annotation Shiny app
│   └── index.html                # App entry point
│
├── scripts/                      # Build and integration scripts
│   └── shinylive_embedded_jekyll_template/  # Custom shinylive template
│
├── 404.html                      # Custom 404 error page
├── CNAME                         # Custom domain configuration
├── index.md                      # Site homepage
│
├── ARCHITECTURE.md               # Architecture documentation
├── CONTRIBUTING.md               # Contribution guidelines
├── README.md                     # Project overview
├── STRUCTURE.md                  # This file
│
├── Gemfile                       # Ruby dependencies
└── Gemfile.lock                  # Locked Ruby dependency versions
```

## Directory Details

### `.github/workflows/`
GitHub Actions workflow definitions for automation:

- **sync_multilingual_content.yml**: Automatically syncs content from multilingual_journal_club repository
  - Runs daily at 6 AM UTC
  - Can be triggered manually
  - Can be triggered by webhook from multilingual_journal_club
  
- **update_iNatle.yml**: Updates the iNatle Shiny application
  - Manual trigger only
  - Clones iNatle repo, exports with shinylive, and deploys

### `_config.yml`
Main Jekyll configuration file with settings for:
- Site metadata (title, author, description)
- Theme configuration (Minima)
- Plugin configuration (jekyll-feed)
- Collection definitions (multilingual_posts)
- Integration settings (multilingual_journal_club)

### `_data/`
Structured data files used across the site:
- **authors.yml**: Author profiles referenced in posts

### `_includes/`
Reusable HTML/Liquid components:
- Included in layouts or pages with `{% include component.html %}`
- Keep DRY (Don't Repeat Yourself) principles

### `_layouts/`
Page layout templates defining the structure of different page types:
- Layouts use Liquid templating
- Inherit from theme or custom layouts
- Override theme defaults when placed here

### `_pages/`
Static pages that aren't blog posts:
- About page
- Author profiles
- Special features (iNatle embedding)

### `_posts/`
**Standard blog posts** written directly in this repository:
- Format: `YYYY-MM-DD-title.md`
- Regular Jekyll posts
- General biology, research, or site meta topics

### `_multilingual_posts/`
**Multilingual scientific content** synced from multilingual_journal_club:
- Automatically populated by GitHub Actions
- Contains translations and multilingual articles
- Do NOT edit directly (edit in multilingual_journal_club instead)
- Collection configured in _config.yml

### `_multilingual_content/` (Optional)
Git submodule linking to multilingual_journal_club:
- Links to specific commit of multilingual_journal_club
- Provides version control of content dependencies
- Alternative to direct cloning in GitHub Actions

### `assets/`
Static assets for the website:
- **css/**: Custom stylesheets
- **images/**: Images used in posts and pages
- **js/**: Custom JavaScript (e.g., Mastodon widget)

### `iNatle_raw/`
Deployed iNatle Shiny application:
- Generated by GitHub Actions from iNatle repository
- Uses shinylive for browser-based R execution
- Custom Jekyll frontmatter for integration
- Modified JavaScript for URL parameter forwarding

### `photo_annotator/`
Photo annotation Shiny application:
- Interactive tool for annotating images
- Embedded in Jekyll site

### `scripts/`
Build and integration scripts:
- **shinylive_embedded_jekyll_template/**: Custom template for exporting Shiny apps with Jekyll-compatible frontmatter
- Other utility scripts for automation

## Content Flow

### Regular Blog Posts
```
Contributor writes in _posts/
    ↓
Commits to thecnidaegritty
    ↓
Automatic GitHub Pages deployment
    ↓
Live on thecnidaegritty.org
```

### Multilingual Content
```
Contributor writes in multilingual_journal_club repo
    ↓
Commits to multilingual_journal_club
    ↓
Webhook/schedule triggers GitHub Actions
    ↓
Content synced to _multilingual_posts/
    ↓
Automatic GitHub Pages deployment
    ↓
Live on thecnidaegritty.org/multilingual/
```

### iNatle Updates
```
Developer updates iNatle repository
    ↓
Manual trigger of GitHub Actions workflow
    ↓
iNatle cloned and exported with shinylive
    ↓
Deployed to iNatle_raw/
    ↓
Automatic GitHub Pages deployment
    ↓
Live on thecnidaegritty.org/iNatle/
```

## File Naming Conventions

### Blog Posts
- **Format**: `YYYY-MM-DD-title-with-hyphens.md`
- **Location**: `_posts/` (regular) or `_multilingual_posts/` (synced)
- **Example**: `2024-01-15-understanding-coral-reefs.md`

### Static Pages
- **Format**: `page-name.md`
- **Location**: `_pages/`
- **Example**: `about.md`

### Data Files
- **Format**: `datatype.yml`
- **Location**: `_data/`
- **Example**: `authors.yml`

## Configuration Files

### Jekyll Configuration
- **_config.yml**: Main configuration
- **Gemfile**: Ruby gem dependencies
- **Gemfile.lock**: Locked versions

### Git Configuration
- **.gitignore**: Files to ignore in version control
- **.gitmodules**: Submodule definitions (if used)

### Deployment Configuration
- **CNAME**: Custom domain (thecnidaegritty.org)
- **404.html**: Custom error page

## Build Artifacts (Excluded from Git)

These are generated during build and excluded from version control:

- `_site/`: Jekyll-generated static site
- `.jekyll-cache/`: Jekyll build cache
- `.jekyll-metadata`: Jekyll metadata
- `.sass-cache/`: Sass compilation cache

## Best Practices

### Where to Edit
- **This repository**: Web design, layouts, Jekyll config, general blog posts
- **multilingual_journal_club**: Multilingual scientific articles and translations
- **iNatle**: iNatle app functionality

### What NOT to Edit
- `_multilingual_posts/*`: Synced content (edit source instead)
- `iNatle_raw/*`: Generated content (edit source instead)
- Build artifacts: Automatically generated

### Adding New Content
- **Blog post**: Add to `_posts/`
- **Static page**: Add to `_pages/`
- **Author**: Add to `_data/authors.yml`
- **Multilingual article**: Add to multilingual_journal_club repo

### Customizing
- **Styles**: Override in `assets/`
- **Layouts**: Override in `_layouts/`
- **Components**: Add to `_includes/`

## Integration Points

### GitHub Actions Integration
- Workflow files: `.github/workflows/`
- Automated syncing and deployment
- Triggered by schedules, manual dispatch, or webhooks

### Git Submodule Integration (Optional)
- Configuration: `.gitmodules`
- Content link: `_multilingual_content/`
- Update with: `git submodule update --remote`

### Jekyll Collections Integration
- Definition: `_config.yml` (collections)
- Content: `_multilingual_posts/`
- Templates: Use in layouts/includes

## Maintenance

### Regular Updates
- **Dependencies**: `bundle update` (monthly)
- **Submodules**: `git submodule update --remote` (automatic via Actions)
- **Content sync**: Automatic daily at 6 AM UTC

### Monitoring
- Check GitHub Actions logs for sync status
- Review deployed site for content updates
- Monitor build times and performance

## Documentation Files

- **README.md**: Project overview and quick start
- **ARCHITECTURE.md**: Technical architecture and design decisions
- **CONTRIBUTING.md**: Contribution guidelines and workflows
- **STRUCTURE.md**: This file - repository structure reference

---

For more information, see:
- [README.md](README.md) for project overview
- [ARCHITECTURE.md](ARCHITECTURE.md) for technical details
- [CONTRIBUTING.md](CONTRIBUTING.md) for contribution guidelines
