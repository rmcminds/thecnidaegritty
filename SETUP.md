# Setup Guide for The Cnidae Gritty

This guide will help you set up a local development environment for The Cnidae Gritty Jekyll blog.

## Prerequisites

Before you begin, ensure you have the following installed:

### Required Software

1. **Ruby** (version 2.7 or higher)
   - Check version: `ruby --version`
   - Installation: 
     - macOS: `brew install ruby` (via Homebrew)
     - Linux: `sudo apt-get install ruby-full` (Debian/Ubuntu)
     - Windows: [RubyInstaller](https://rubyinstaller.org/)

2. **RubyGems** (usually comes with Ruby)
   - Check version: `gem --version`

3. **Bundler**
   - Install: `gem install bundler`
   - Check version: `bundler --version`

4. **Git**
   - Check version: `git --version`
   - Installation: [git-scm.com](https://git-scm.com/downloads)

### Optional (for full integration testing)

- **R** (for iNatle development): [r-project.org](https://www.r-project.org/)
- **GitHub CLI** (for easier GitHub operations): [cli.github.com](https://cli.github.com/)

## Quick Setup

### 1. Clone the Repository

```bash
# Clone the main repository
git clone https://github.com/rmcminds/thecnidaegritty.git

# Navigate to the repository
cd thecnidaegritty
```

### 2. Install Dependencies

```bash
# Install Ruby gems defined in Gemfile
bundle install
```

This will install:
- Jekyll (static site generator)
- GitHub Pages gem (ensures compatibility)
- Jekyll plugins (jekyll-feed, etc.)
- All required dependencies

### 3. Serve the Site Locally

```bash
# Start Jekyll development server
bundle exec jekyll serve

# Or with live reload (automatically refreshes browser on changes)
bundle exec jekyll serve --livereload
```

The site will be available at: `http://localhost:4000`

### 4. Make Changes

- Edit files in your preferred text editor
- Save changes
- Jekyll will automatically rebuild (with `--livereload`)
- Refresh browser to see updates

## Advanced Setup: With Submodules

If you want to test the multilingual content integration locally:

### Option A: Using Git Submodules

```bash
# Clone with submodules
git clone --recurse-submodules https://github.com/rmcminds/thecnidaegritty.git

# Or if already cloned, initialize submodules
git submodule update --init --recursive
```

### Option B: Manual Clone for Testing

```bash
# Create the multilingual content directory
mkdir -p _multilingual_content

# Clone multilingual_journal_club into it
git clone https://github.com/rmcminds/multilingual_journal_club.git _multilingual_content

# Copy content to Jekyll collection
mkdir -p _multilingual_posts
cp -r _multilingual_content/_posts/* _multilingual_posts/ 2>/dev/null || true
```

## Project Structure

After setup, your directory should look like:

```
thecnidaegritty/
├── _config.yml              # Main configuration
├── _data/                   # Site data
├── _includes/               # Reusable components
├── _layouts/                # Page layouts
├── _pages/                  # Static pages
├── _posts/                  # Your blog posts
├── _multilingual_posts/     # Synced multilingual content
├── assets/                  # CSS, JS, images
├── Gemfile                  # Ruby dependencies
└── ... (other files)
```

## Common Tasks

### Creating a New Blog Post

```bash
# Create a new file in _posts directory
# Format: YYYY-MM-DD-title-with-hyphens.md

touch _posts/2024-01-15-my-new-post.md
```

Add frontmatter:
```yaml
---
layout: post
title: "My New Post"
date: 2024-01-15 10:00:00 -0400
categories: biology
author: rmcminds
excerpt_separator: <!--more-->
---

Your content here...<!--more-->

More content after the excerpt...
```

### Testing Changes

```bash
# Start Jekyll server
bundle exec jekyll serve --livereload

# Visit http://localhost:4000 in your browser

# Check for errors in terminal
# Verify your changes in the browser
```

### Updating Dependencies

```bash
# Update all gems to latest compatible versions
bundle update

# Update a specific gem
bundle update jekyll

# Check for outdated gems
bundle outdated
```

### Building the Site (Production Build)

```bash
# Build site for production (creates _site directory)
bundle exec jekyll build

# Build and check for issues
bundle exec jekyll build --verbose
```

## Troubleshooting

### Issue: "Ruby version too old"

**Solution**: Install a newer Ruby version
```bash
# Check current version
ruby --version

# Install newer version (macOS with rbenv)
brew install rbenv
rbenv install 3.1.0
rbenv global 3.1.0
```

### Issue: "Bundle install fails"

**Solution**: Clear cache and reinstall
```bash
# Remove Gemfile.lock
rm Gemfile.lock

# Clear bundle cache
bundle clean --force

# Install again
bundle install
```

### Issue: "Jekyll serve fails with port error"

**Solution**: Port 4000 is already in use
```bash
# Use a different port
bundle exec jekyll serve --port 4001

# Or kill existing process
lsof -ti:4000 | xargs kill -9
```

### Issue: "Changes not showing up"

**Solutions**:
1. Hard refresh browser (Ctrl+Shift+R / Cmd+Shift+R)
2. Clear Jekyll cache:
   ```bash
   bundle exec jekyll clean
   bundle exec jekyll serve
   ```
3. Restart Jekyll server
4. Check file is in correct directory
5. Verify frontmatter is valid YAML

### Issue: "Submodule not updating"

**Solution**: Manually update submodule
```bash
# Update to latest commit
git submodule update --remote

# Pull latest changes
cd _multilingual_content
git pull origin main
cd ..
```

## IDE/Editor Setup

### Visual Studio Code

**Recommended Extensions**:
- Jekyll Syntax Support
- YAML
- Markdown All in One
- Liquid

**Settings** (.vscode/settings.json):
```json
{
  "files.associations": {
    "*.html": "liquid"
  },
  "liquid.format.enable": true
}
```

### Atom

**Recommended Packages**:
- language-liquid
- jekyll
- markdown-preview-enhanced

### Vim/Neovim

**Plugins**:
- vim-liquid
- vim-markdown
- vim-jekyll

## Jekyll Commands Reference

```bash
# Serve site locally
bundle exec jekyll serve

# Serve with drafts
bundle exec jekyll serve --drafts

# Serve with future posts
bundle exec jekyll serve --future

# Serve with live reload
bundle exec jekyll serve --livereload

# Build site only
bundle exec jekyll build

# Clean generated files
bundle exec jekyll clean

# Show Jekyll version
bundle exec jekyll --version

# Get help
bundle exec jekyll help
```

## Git Workflow

### Basic Workflow

```bash
# Check status
git status

# Create a new branch for your changes
git checkout -b my-feature-branch

# Make changes to files
# ... edit files ...

# Stage changes
git add .

# Commit changes
git commit -m "Add new blog post about coral reefs"

# Push to GitHub
git push origin my-feature-branch

# Create pull request on GitHub
```

### Keeping Branch Up to Date

```bash
# Switch to main branch
git checkout main

# Pull latest changes
git pull origin main

# Switch back to your branch
git checkout my-feature-branch

# Merge main into your branch
git merge main
```

## Testing Integration Features

### Test Multilingual Content Sync

```bash
# Clone multilingual_journal_club
git clone https://github.com/rmcminds/multilingual_journal_club.git _multilingual_content

# Copy posts to collection
cp -r _multilingual_content/_posts/* _multilingual_posts/

# Serve and verify
bundle exec jekyll serve
```

### Test iNatle Integration

The iNatle integration requires R and specific packages. See `.github/workflows/update_iNatle.yml` for the full process.

For local testing, the easiest approach is to verify the existing deployed version works correctly.

## Additional Resources

### Jekyll Documentation
- [Jekyll Official Docs](https://jekyllrb.com/docs/)
- [Jekyll Themes](https://jekyllrb.com/docs/themes/)
- [Liquid Templating](https://shopify.github.io/liquid/)

### GitHub Pages
- [GitHub Pages Docs](https://docs.github.com/en/pages)
- [Dependency Versions](https://pages.github.com/versions/)

### Project Documentation
- [README.md](README.md): Project overview
- [ARCHITECTURE.md](ARCHITECTURE.md): Technical architecture
- [CONTRIBUTING.md](CONTRIBUTING.md): Contribution guidelines
- [STRUCTURE.md](STRUCTURE.md): Repository structure

## Getting Help

If you encounter issues:

1. Check this guide's troubleshooting section
2. Review project documentation (README.md, CONTRIBUTING.md)
3. Search existing GitHub issues
4. Open a new issue with details:
   - What you tried to do
   - What happened
   - Error messages
   - Your environment (OS, Ruby version, etc.)

## Next Steps

After setup:

1. Read [CONTRIBUTING.md](CONTRIBUTING.md) for contribution guidelines
2. Review [ARCHITECTURE.md](ARCHITECTURE.md) for technical details
3. Explore existing posts in `_posts/` for examples
4. Start contributing!

---

Happy coding! 🦑
