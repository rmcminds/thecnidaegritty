# Integration Guide: multilingual_journal_club

This guide explains how to set up and manage the integration between The Cnidae Gritty blog and the multilingual_journal_club repository.

## Overview

The Cnidae Gritty can integrate content from the multilingual_journal_club repository using two approaches:

1. **Git Submodules** (Recommended): Links repositories via Git
2. **GitHub Actions Sync** (Alternative): Clones and syncs via automation

This guide covers both approaches.

## Approach 1: Git Submodules (Recommended)

### Why Use Submodules?

- Version control of content dependencies
- Easy local development and testing
- Clear content provenance
- Git-native integration

### Initial Setup

#### Step 1: Add the Submodule

From the root of thecnidaegritty repository:

```bash
# Add multilingual_journal_club as a submodule
git submodule add https://github.com/rmcminds/multilingual_journal_club.git _multilingual_content

# Commit the submodule addition
git add .gitmodules _multilingual_content
git commit -m "Add multilingual_journal_club as submodule"
git push
```

#### Step 2: Update .gitignore

Uncomment the submodule line in `.gitignore`:

```gitignore
# Submodule content (tracked separately)
_multilingual_content/  # Uncomment if using git submodule
```

Should become:

```gitignore
# Submodule content (tracked separately)
_multilingual_content/  # Git submodule content
```

#### Step 3: Test the Integration Locally

```bash
# Copy content from submodule to Jekyll collection
mkdir -p _multilingual_posts
cp -r _multilingual_content/_posts/* _multilingual_posts/ 2>/dev/null || echo "No posts yet"

# Build and test
bundle exec jekyll serve

# Visit http://localhost:4000 to see the integrated content
```

### Working with Submodules

#### Cloning the Repository (Contributors)

When cloning thecnidaegritty with submodules:

```bash
# Option 1: Clone with submodules
git clone --recurse-submodules https://github.com/rmcminds/thecnidaegritty.git

# Option 2: Clone first, then initialize submodules
git clone https://github.com/rmcminds/thecnidaegritty.git
cd thecnidaegritty
git submodule update --init --recursive
```

#### Updating Submodule Content

To pull latest changes from multilingual_journal_club:

```bash
# Update submodule to latest commit
git submodule update --remote _multilingual_content

# Review changes
cd _multilingual_content
git log -1
cd ..

# Copy updated content to Jekyll collection
cp -r _multilingual_content/_posts/* _multilingual_posts/

# Commit the submodule update
git add _multilingual_content _multilingual_posts
git commit -m "Update multilingual_journal_club content"
git push
```

#### Automated Updates

The GitHub Actions workflow `.github/workflows/sync_multilingual_content.yml` handles automatic updates:

- **Scheduled**: Daily at 6 AM UTC
- **Manual**: Via workflow dispatch
- **Webhook** (future): Triggered by multilingual_journal_club commits

### Submodule Best Practices

1. **Always commit submodule updates**: Track which version of multilingual_journal_club you're using
2. **Test locally before pushing**: Ensure content builds correctly
3. **Document major updates**: Note significant content changes in commit messages
4. **Use workflow dispatch**: Manually trigger sync after major content updates

## Approach 2: GitHub Actions Sync (Alternative)

### Why Use GitHub Actions Sync?

- No local submodule management
- Simpler for contributors who only work on blog posts
- Can transform content during sync
- Works without Git submodule knowledge

### How It Works

The workflow `.github/workflows/sync_multilingual_content.yml`:

1. Clones multilingual_journal_club repository
2. Copies posts to `_multilingual_posts/`
3. Adds source metadata to posts
4. Commits and pushes changes

### Manual Trigger

To manually sync content:

1. Go to GitHub repository: `https://github.com/rmcminds/thecnidaegritty`
2. Click **Actions** tab
3. Select **Sync Multilingual Content** workflow
4. Click **Run workflow** button
5. Confirm and run

### Automated Sync

The workflow runs automatically:
- **Daily** at 6 AM UTC
- Pulls latest content from multilingual_journal_club
- Commits changes if new content found

## Webhook Integration (Future Enhancement)

### Goal

Automatically sync when multilingual_journal_club receives new commits.

### Setup (When Ready)

#### In multilingual_journal_club Repository:

1. Go to **Settings** → **Webhooks** → **Add webhook**
2. **Payload URL**: `https://api.github.com/repos/rmcminds/thecnidaegritty/dispatches`
3. **Content type**: `application/json`
4. **Secret**: Generate a secure token
5. **Events**: Select "Just the push event"
6. **Active**: ✓ Check

#### In thecnidaegritty Repository:

Add repository secret:
1. Go to **Settings** → **Secrets and variables** → **Actions**
2. Add secret: `WEBHOOK_SECRET` with the generated token

Update workflow to validate webhook secret.

## Content Management

### Content Flow

```
multilingual_journal_club
        ↓
    (submodule/clone)
        ↓
_multilingual_content
        ↓
    (copy + metadata)
        ↓
_multilingual_posts
        ↓
    (Jekyll build)
        ↓
thecnidaegritty.org/multilingual/
```

### File Structure

```
thecnidaegritty/
├── _multilingual_content/          # Submodule (source)
│   ├── _posts/
│   │   └── 2024-01-15-paper-title.md
│   └── translations/
└── _multilingual_posts/             # Jekyll collection (built)
    └── 2024-01-15-paper-title.md   # + metadata
```

### Post Metadata

Posts synced from multilingual_journal_club automatically receive:

```yaml
---
# Original frontmatter from multilingual_journal_club
layout: post
title: "Understanding Coral Symbiosis"
date: 2024-01-15 10:00:00 -0400
categories: research
author: rmcminds

# Added during sync
source_repo: multilingual_journal_club
---
```

### URL Structure

Multilingual posts appear at:
```
https://thecnidaegritty.org/multilingual/2024/01/15/paper-title/
```

Regular blog posts appear at:
```
https://thecnidaegritty.org/blog-meta/2024/01/15/post-title/
```

## Troubleshooting

### Issue: Submodule is empty

**Problem**: `_multilingual_content/` exists but is empty

**Solution**:
```bash
git submodule update --init --recursive
```

### Issue: Submodule won't update

**Problem**: `git submodule update --remote` doesn't pull latest changes

**Solution**:
```bash
cd _multilingual_content
git fetch origin
git checkout main
git pull origin main
cd ..
git add _multilingual_content
git commit -m "Update submodule to latest"
```

### Issue: Build fails with submodule

**Problem**: Jekyll tries to process submodule content

**Solution**: Ensure `_multilingual_content` is in `_config.yml` exclude list:
```yaml
exclude:
  - _multilingual_content
```

### Issue: Content not appearing on site

**Problem**: Posts in `_multilingual_posts/` don't show up

**Solutions**:
1. Check Jekyll collection is defined in `_config.yml`
2. Verify post filename format: `YYYY-MM-DD-title.md`
3. Ensure valid frontmatter
4. Check post date (future posts don't show by default)

### Issue: Sync workflow fails

**Problem**: GitHub Actions workflow errors

**Solutions**:
1. Check workflow logs in Actions tab
2. Verify repository permissions
3. Ensure multilingual_journal_club is accessible
4. Check for file permission issues

## Testing Integration

### Local Testing Checklist

- [ ] Submodule initialized and updated
- [ ] Content copied to `_multilingual_posts/`
- [ ] Jekyll build succeeds
- [ ] Multilingual posts appear on home page
- [ ] Multilingual post URLs work
- [ ] Post metadata is correct

### Testing Commands

```bash
# Update submodule
git submodule update --remote

# Copy content
cp -r _multilingual_content/_posts/* _multilingual_posts/

# Clean and rebuild
bundle exec jekyll clean
bundle exec jekyll build

# Check for errors
echo $?  # Should be 0

# Serve locally
bundle exec jekyll serve

# Test in browser
open http://localhost:4000
```

## Maintenance Tasks

### Weekly

- [ ] Review automated sync logs
- [ ] Check for failed workflows
- [ ] Verify content is up to date

### Monthly

- [ ] Update dependencies: `bundle update`
- [ ] Review submodule version
- [ ] Check for new multilingual_journal_club content

### Quarterly

- [ ] Review integration approach effectiveness
- [ ] Gather contributor feedback
- [ ] Consider improvements or optimizations

## Best Practices

### For Content Contributors

**Adding content to multilingual_journal_club**:
1. Write content in multilingual_journal_club repository
2. Follow that repository's contribution guidelines
3. Content will automatically sync to blog
4. Allow 24 hours for scheduled sync, or request manual trigger

### For Blog Maintainers

**Managing the integration**:
1. Monitor GitHub Actions for sync failures
2. Manually trigger sync after major content updates
3. Keep submodule updated regularly
4. Test locally before merging integration changes

### For Developers

**Modifying the integration**:
1. Test changes locally with sample content
2. Update documentation when changing workflow
3. Maintain backward compatibility
4. Coordinate with multilingual_journal_club maintainers

## Getting Help

### Documentation

- [README.md](README.md): Project overview
- [ARCHITECTURE.md](ARCHITECTURE.md): Technical architecture
- [CONTRIBUTING.md](CONTRIBUTING.md): Contribution guidelines

### Support Channels

- **Integration issues**: Open issue in thecnidaegritty repository
- **Content issues**: Open issue in multilingual_journal_club repository
- **General questions**: GitHub Discussions

### Contact

- **Email**: r.mcminds@thecnidaegritty.org
- **Twitter**: @thecnidaegritty
- **Mastodon**: @thecnidaegritty@sciencemastodon.com

---

## Quick Reference

### Common Commands

```bash
# Clone with submodules
git clone --recurse-submodules https://github.com/rmcminds/thecnidaegritty.git

# Initialize submodules (if not cloned with --recurse-submodules)
git submodule update --init --recursive

# Update submodule to latest
git submodule update --remote _multilingual_content

# Copy content to Jekyll collection
cp -r _multilingual_content/_posts/* _multilingual_posts/

# Build site
bundle exec jekyll build

# Serve site locally
bundle exec jekyll serve

# Clean build artifacts
bundle exec jekyll clean
```

### File Locations

- **Submodule**: `_multilingual_content/`
- **Jekyll collection**: `_multilingual_posts/`
- **Sync workflow**: `.github/workflows/sync_multilingual_content.yml`
- **Jekyll config**: `_config.yml`
- **Collection settings**: `_config.yml` → `collections` → `multilingual_posts`

---

*For detailed architectural information, see [ARCHITECTURE.md](ARCHITECTURE.md)*
