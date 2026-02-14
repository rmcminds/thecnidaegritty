# Architecture Documentation

## Overview

**The Cnidae Gritty** is designed as a modular, integrated blogging platform that separates content creation from web presentation. This architecture enables focused contribution workflows while maintaining a cohesive user experience.

## Design Principles

### 1. Separation of Concerns
- **The Cnidae Gritty** (this repository): Web development, Jekyll configuration, hosting, and presentation layer
- **multilingual_journal_club**: Language-focused content creation, translations, and scientific communication
- **iNatle**: Interactive R/Shiny application development

### 2. Clear Responsibilities

| Repository | Purpose | Contributors Focus On |
|------------|---------|----------------------|
| **thecnidaegritty** | Jekyll blog, web interface, deployment | Web development, theme customization, site structure |
| **multilingual_journal_club** | Multilingual scientific articles | Content writing, translation, scientific communication |
| **iNatle** | Interactive word game app | R/Shiny development, game mechanics |

### 3. Automated Integration
- Content flows from source repositories to the blog automatically
- No manual synchronization required
- Contributors work in their specialized repositories

## System Architecture

```
┌─────────────────────────────────────────────────────────────┐
│                    The Cnidae Gritty                        │
│                 (Jekyll Blog Platform)                       │
│                                                              │
│  ┌────────────┐  ┌──────────────┐  ┌──────────────┐       │
│  │  Jekyll    │  │   GitHub     │  │   GitHub     │       │
│  │  Engine    │  │   Pages      │  │   Actions    │       │
│  └────────────┘  └──────────────┘  └──────────────┘       │
│         │               │                   │               │
│         └───────────────┴───────────────────┘               │
│                         │                                    │
└─────────────────────────┼────────────────────────────────────┘
                          │
        ┌─────────────────┼─────────────────┐
        │                 │                 │
        ▼                 ▼                 ▼
┌───────────────┐  ┌────────────────┐  ┌────────────┐
│ multilingual_ │  │     iNatle     │  │   Custom   │
│ journal_club  │  │   (Submodule/  │  │   Posts    │
│  (Submodule)  │  │    Clone)      │  │            │
└───────────────┘  └────────────────┘  └────────────┘
```

## Integration Approaches

### Approach 1: Git Submodules (Recommended for multilingual_journal_club)

**Advantages**:
- Direct Git integration
- Version control of content dependencies
- Easy local development
- Clear content provenance

**Implementation**:
```bash
# Add multilingual_journal_club as a submodule
git submodule add https://github.com/rmcminds/multilingual_journal_club.git _multilingual_content

# Content is linked, not duplicated
# Jekyll reads from _multilingual_content/_posts/
```

**Workflow**:
1. Submodule tracks a specific commit of multilingual_journal_club
2. GitHub Actions workflow updates submodule and rebuilds site
3. Jekyll processes content from submodule directory

### Approach 2: GitHub Actions Clone and Sync (Current: iNatle)

**Advantages**:
- Build-time integration
- Can transform content during sync
- No local submodule management needed
- Works well for compiled applications

**Implementation**:
```yaml
# .github/workflows/sync_multilingual_content.yml
- name: Clone multilingual_journal_club
  run: git clone https://github.com/rmcminds/multilingual_journal_club.git

- name: Copy content to Jekyll
  run: |
    cp -r multilingual_journal_club/_posts/* _multilingual_posts/
    # Transform/process content as needed
```

**Current Usage**: iNatle repository (R/Shiny app compiled to static assets)

### Hybrid Approach (Recommended)

Combine both methods:
1. **Submodule** for multilingual_journal_club (content files)
2. **GitHub Actions Clone** for iNatle (compiled application)

**Rationale**:
- Content benefits from Git submodule tracking
- Applications need compilation and can't be directly served as submodules
- Each integration method suits its content type

## Repository Integration Details

### multilingual_journal_club Integration

#### Directory Structure
```
thecnidaegritty/
├── _multilingual_content/        # Git submodule
│   ├── _posts/                   # Source posts
│   ├── translations/             # Translation files
│   └── README.md
└── _multilingual_posts/          # Jekyll collection (symlink or copy)
```

#### Jekyll Configuration
```yaml
# _config.yml
collections:
  multilingual_posts:
    output: true
    permalink: /multilingual/:year/:month/:day/:title/

defaults:
  - scope:
      path: "_multilingual_posts"
      type: "multilingual_posts"
    values:
      layout: "post"
      source_repo: "multilingual_journal_club"
```

#### Sync Workflow
```yaml
# .github/workflows/sync_multilingual_content.yml
name: Sync Multilingual Content

on:
  schedule:
    - cron: '0 6 * * *'  # Daily at 6 AM UTC
  workflow_dispatch:      # Manual trigger
  repository_dispatch:    # Triggered by multilingual_journal_club webhook

jobs:
  sync:
    runs-on: ubuntu-latest
    steps:
      - name: Checkout
        uses: actions/checkout@v3
        with:
          submodules: true
      
      - name: Update submodule
        run: |
          git submodule update --remote _multilingual_content
          cp -r _multilingual_content/_posts/* _multilingual_posts/
      
      - name: Commit changes
        run: |
          git add .
          git commit -m "Sync multilingual content" || echo "No changes"
          git push
```

### iNatle Integration (Existing)

**Current Workflow**: `.github/workflows/update_iNatle.yml`

**Process**:
1. Clones iNatle repository
2. Installs R and dependencies
3. Exports Shiny app using shinylive
4. Applies custom Jekyll template
5. Modifies JavaScript for parameter forwarding
6. Commits to `iNatle_raw/` directory

**Key Features**:
- Custom shinylive template for Jekyll frontmatter
- URL parameter forwarding to iframe
- Browser-based R execution (no server needed)

## Content Flow

### Multilingual Content Workflow

```
┌──────────────────────────────┐
│  Contributor writes content  │
│  in multilingual_journal_club│
└──────────────┬───────────────┘
               │
               ▼
┌──────────────────────────────┐
│  Commits to multilingual_    │
│  journal_club repository     │
└──────────────┬───────────────┘
               │
               ▼ (webhook/scheduled)
┌──────────────────────────────┐
│  GitHub Actions in           │
│  thecnidaegritty triggered   │
└──────────────┬───────────────┘
               │
               ▼
┌──────────────────────────────┐
│  Submodule updated,          │
│  content synced              │
└──────────────┬───────────────┘
               │
               ▼
┌──────────────────────────────┐
│  Jekyll rebuilds site        │
│  (GitHub Pages)              │
└──────────────┬───────────────┘
               │
               ▼
┌──────────────────────────────┐
│  New content live on         │
│  thecnidaegritty.org         │
└──────────────────────────────┘
```

### Blog Post Workflow (Direct Contributions)

```
┌──────────────────────────────┐
│  Contributor creates post    │
│  in _posts/ directory        │
└──────────────┬───────────────┘
               │
               ▼
┌──────────────────────────────┐
│  Commits to thecnidaegritty  │
└──────────────┬───────────────┘
               │
               ▼
┌──────────────────────────────┐
│  GitHub Pages auto-deploys   │
└──────────────┬───────────────┘
               │
               ▼
┌──────────────────────────────┐
│  Post live on site           │
└──────────────────────────────┘
```

## Build and Deployment

### Local Development
```bash
# Clone with submodules
git clone --recurse-submodules https://github.com/rmcminds/thecnidaegritty.git

# Update submodules
git submodule update --remote

# Install Jekyll dependencies
bundle install

# Serve locally
bundle exec jekyll serve --livereload
```

### Production Deployment

**Platform**: GitHub Pages

**Trigger**: Push to `main` branch

**Process**:
1. GitHub Pages detects changes
2. Runs Jekyll build
3. Deploys to GitHub CDN
4. Available at thecnidaegritty.org

**Custom Domain**: Configured via CNAME file

## Security Considerations

### Content Validation
- All content from submodules is treated as trusted
- Review process for multilingual_journal_club submissions
- No dynamic content execution (static site)

### Secrets Management
- No API keys or secrets in repository
- GitHub Actions uses built-in GITHUB_TOKEN
- No external service credentials needed

### Dependency Management
- Jekyll plugins via Gemfile
- Regular dependency updates
- GitHub Dependabot enabled

## Scalability Considerations

### Performance
- Static site: Excellent performance
- CDN delivery via GitHub Pages
- No server-side processing

### Content Volume
- Jekyll handles thousands of posts efficiently
- Submodule approach scales well
- Build time increases linearly with content

### Maintenance
- Automated sync reduces manual work
- Clear separation reduces cognitive load
- Modular design enables independent updates

## Future Enhancements

### Potential Improvements

1. **Webhook Integration**
   - Direct webhook from multilingual_journal_club to trigger sync
   - Faster content propagation
   - Reduced latency

2. **Content Transformation Pipeline**
   - Automated image optimization
   - Language-specific formatting
   - SEO optimization

3. **Preview Environments**
   - Branch-based preview deployments
   - Test multilingual content before publish
   - Review workflow integration

4. **Analytics Integration**
   - Track multilingual content engagement
   - Understand language preferences
   - Guide content strategy

5. **Advanced Jekyll Features**
   - Custom collections per language
   - Language switcher component
   - Translated navigation

## Troubleshooting

### Common Issues

**Submodule not updating**:
```bash
git submodule update --remote --merge
git commit -am "Update submodule"
git push
```

**Build failures**:
- Check Jekyll version compatibility
- Verify Gemfile.lock
- Review GitHub Pages build logs

**Content not appearing**:
- Verify collection configuration in _config.yml
- Check file naming conventions (YYYY-MM-DD-title.md)
- Ensure frontmatter is valid YAML

## Maintenance

### Regular Tasks

**Weekly**:
- Review automated sync logs
- Check for failed workflows
- Monitor site performance

**Monthly**:
- Update dependencies (`bundle update`)
- Review and merge Dependabot PRs
- Audit submodule versions

**Quarterly**:
- Review architecture effectiveness
- Gather contributor feedback
- Plan enhancements

## Conclusion

This architecture provides:
- ✅ Clear separation of concerns
- ✅ Automated content synchronization
- ✅ Scalable, maintainable design
- ✅ Focused contribution workflows
- ✅ Reliable deployment pipeline

Contributors can focus on their strengths (content vs. web development) while the system handles integration automatically.
