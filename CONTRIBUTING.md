# Contributing to The Cnidae Gritty

Thank you for your interest in contributing to The Cnidae Gritty! This guide will help you understand how to contribute effectively based on the type of contribution you want to make.

## 🎯 Quick Guide: Where Should I Contribute?

| What You Want to Do | Where to Contribute | Repository |
|---------------------|---------------------|------------|
| Write multilingual scientific articles | multilingual_journal_club | [rmcminds/multilingual_journal_club](https://github.com/rmcminds/multilingual_journal_club) |
| Translate existing scientific content | multilingual_journal_club | [rmcminds/multilingual_journal_club](https://github.com/rmcminds/multilingual_journal_club) |
| Write general blog posts | The Cnidae Gritty | This repository |
| Improve site design/layout | The Cnidae Gritty | This repository |
| Fix Jekyll configuration | The Cnidae Gritty | This repository |
| Work on iNatle game | iNatle | [rmcminds/iNatle](https://github.com/rmcminds/iNatle) |
| Report website bugs | The Cnidae Gritty | This repository |

## 📝 Contributing Blog Posts

### Direct Blog Posts (This Repository)

For general blog posts about biology, research, or other topics:

1. **Create a new file** in `_posts/` directory:
   ```
   _posts/YYYY-MM-DD-your-post-title.md
   ```

2. **Add frontmatter**:
   ```yaml
   ---
   layout: post
   title: "Your Post Title"
   date: YYYY-MM-DD HH:MM:SS -0400
   categories: your-category
   author: your-author-id
   excerpt_separator: <!--more-->
   ---
   ```

3. **Write your content** using Markdown:
   ```markdown
   Your introduction paragraph goes here.<!--more-->
   
   The rest of your post continues here...
   ```

4. **Add your author info** to `_data/authors.yml` (if not already present):
   ```yaml
   your-author-id:
     name: Your Name
     bio: Brief bio about yourself
     twitter: your_twitter_handle  # optional
     email: your.email@example.com  # optional
   ```

5. **Test locally**:
   ```bash
   bundle exec jekyll serve
   # Visit http://localhost:4000
   ```

6. **Submit a pull request** with your changes.

### Multilingual Scientific Articles

For articles about scientific papers, especially with translations:

**⚠️ Important**: Contribute these to the [multilingual_journal_club](https://github.com/rmcminds/multilingual_journal_club) repository, NOT here.

**Why?** The multilingual_journal_club repository is specifically designed for language-focused scientific content. Content from that repository is automatically synchronized to this blog.

**How?**:
1. Go to [rmcminds/multilingual_journal_club](https://github.com/rmcminds/multilingual_journal_club)
2. Follow the contribution guidelines in that repository
3. Your content will automatically appear on thecnidaegritty.org after synchronization

## 🌐 Working with Jekyll

### Prerequisites

- Ruby 2.7 or higher
- Bundler gem (`gem install bundler`)
- Git

### Setting Up Local Development

1. **Clone the repository**:
   ```bash
   git clone https://github.com/rmcminds/thecnidaegritty.git
   cd thecnidaegritty
   ```

2. **Initialize submodules** (for multilingual content):
   ```bash
   git submodule update --init --recursive
   ```

3. **Install dependencies**:
   ```bash
   bundle install
   ```

4. **Run Jekyll locally**:
   ```bash
   bundle exec jekyll serve
   ```
   
   Or with live reload:
   ```bash
   bundle exec jekyll serve --livereload
   ```

5. **Access the site** at `http://localhost:4000`

### Jekyll Project Structure

```
thecnidaegritty/
├── _config.yml           # Site configuration
├── _data/               
│   └── authors.yml       # Author information
├── _includes/            # Reusable components (HTML snippets)
├── _layouts/             # Page layouts
│   ├── default.html
│   ├── home.html
│   ├── page.html
│   └── post.html
├── _pages/               # Static pages
├── _posts/               # Blog posts (standard)
├── _multilingual_posts/  # Multilingual content (synced)
└── assets/               # CSS, JS, images
```

### Post Format

All blog posts must follow Jekyll's naming convention:

**Filename**: `YYYY-MM-DD-title-with-hyphens.md`

**Required Frontmatter**:
```yaml
---
layout: post                    # Required: use 'post' layout
title: "Your Post Title"        # Required: post title
date: 2024-01-15 10:30:00 -0400 # Required: publication date/time
categories: category-name       # Required: post category
author: author-id               # Required: matches _data/authors.yml
excerpt_separator: <!--more-->  # Optional: where excerpt ends
---
```

**Content Structure**:
```markdown
---
layout: post
title: "Understanding Cnidarian Biology"
date: 2024-01-15 10:30:00 -0400
categories: science
author: rmcminds
excerpt_separator: <!--more-->
---

This is the introduction paragraph that will appear as an excerpt in post listings.<!--more-->

## Section Heading

The rest of your content goes here. You can use:
- Markdown formatting
- **Bold** and *italic* text
- [Links](https://example.com)
- Images: ![Alt text](/assets/images/image.jpg)
- Code blocks
- Lists and tables

## Another Section

More content...
```

## 🎨 Contributing Design/Layout Changes

### Theme Customization

The site uses the Minima theme with custom overrides.

**To modify layouts**:
1. Copy the default layout from Minima gem to `_layouts/`
2. Make your changes
3. Test thoroughly on different screen sizes

**To modify styles**:
1. Add custom CSS to `assets/main.scss`
2. Use SCSS variables for consistency
3. Follow existing code style

**To add components**:
1. Create reusable components in `_includes/`
2. Include them in layouts or posts: `{% include component.html %}`

### Testing Changes

1. Test locally with `bundle exec jekyll serve`
2. Check multiple browsers if possible
3. Verify mobile responsiveness
4. Test with sample multilingual content

## 🔄 Working with Integrated Content

### Understanding the Integration

The Cnidae Gritty integrates content from multiple sources:

1. **multilingual_journal_club**: Via git submodule and GitHub Actions
2. **iNatle**: Via GitHub Actions cloning and compilation
3. **Direct posts**: Written directly in this repository

### Syncing Multilingual Content

Content from multilingual_journal_club syncs automatically via:
- **Scheduled**: Daily at 6 AM UTC
- **Manual**: Via GitHub Actions workflow dispatch
- **Webhook**: (Future) Triggered by multilingual_journal_club updates

**Manual sync**:
1. Go to Actions tab in GitHub
2. Select "Sync Multilingual Content" workflow
3. Click "Run workflow"

**Local sync** (for testing):
```bash
# Update submodule to latest
git submodule update --remote _multilingual_content

# Copy content to Jekyll collection
cp -r _multilingual_content/_posts/* _multilingual_posts/

# View changes
bundle exec jekyll serve
```

## 🚀 Deployment

### Automatic Deployment

The site deploys automatically via GitHub Pages:
- **Trigger**: Push to `main` branch
- **Build**: GitHub Pages runs Jekyll
- **Deploy**: Automatically to thecnidaegritty.org

### Manual Deployment Steps

1. Ensure all tests pass locally
2. Commit your changes with descriptive message
3. Push to your branch
4. Create a pull request
5. Wait for review and approval
6. Merge to `main` branch
7. GitHub Pages will deploy automatically

## 🧪 Testing Your Contributions

### Before Submitting a PR

**For blog posts**:
- [ ] Verify frontmatter is correct
- [ ] Check for spelling/grammar errors
- [ ] Ensure images load correctly
- [ ] Test links (internal and external)
- [ ] Verify excerpt displays correctly on home page

**For code changes**:
- [ ] Test locally with `bundle exec jekyll serve`
- [ ] Check browser console for errors
- [ ] Test responsive design
- [ ] Verify no broken links
- [ ] Check that existing posts still work

**For Jekyll configuration**:
- [ ] Verify site builds without errors
- [ ] Test all affected pages
- [ ] Check navigation and menus
- [ ] Ensure backward compatibility

## 📋 Pull Request Guidelines

### Creating a Good PR

1. **Clear title**: Describe what the PR does
   - ✅ "Add blog post about coral symbiosis"
   - ✅ "Fix mobile navigation menu bug"
   - ❌ "Update files"

2. **Detailed description**:
   - What changes were made
   - Why they were necessary
   - How to test the changes
   - Screenshots (for visual changes)

3. **Small, focused changes**:
   - One feature or fix per PR
   - Easier to review and merge
   - Reduces risk of conflicts

4. **Follow conventions**:
   - Match existing code style
   - Use consistent naming
   - Follow Jekyll best practices

### PR Template

```markdown
## Description
Brief description of changes

## Type of Change
- [ ] New blog post
- [ ] Bug fix
- [ ] Design/layout change
- [ ] Configuration change
- [ ] Documentation update

## Testing
- [ ] Tested locally
- [ ] Checked multiple browsers
- [ ] Verified mobile responsiveness
- [ ] No errors in browser console

## Screenshots
(if applicable)

## Additional Notes
Any other relevant information
```

## 🐛 Reporting Bugs

### Where to Report

- **Website bugs**: Open an issue in this repository
- **Multilingual content issues**: [multilingual_journal_club issues](https://github.com/rmcminds/multilingual_journal_club/issues)
- **iNatle bugs**: [iNatle issues](https://github.com/rmcminds/iNatle/issues)

### Bug Report Template

```markdown
**Description**
Clear description of the bug

**Steps to Reproduce**
1. Go to '...'
2. Click on '...'
3. See error

**Expected Behavior**
What should happen

**Actual Behavior**
What actually happens

**Screenshots**
If applicable

**Environment**
- OS: [e.g., macOS, Windows, Linux]
- Browser: [e.g., Chrome, Firefox, Safari]
- Version: [e.g., 22]

**Additional Context**
Any other relevant information
```

## 💡 Feature Requests

We welcome feature suggestions! Please:
1. Check existing issues first
2. Provide clear use case
3. Explain expected benefit
4. Consider implementation complexity

## 🔐 Security

If you discover a security vulnerability:
1. **Do NOT** open a public issue
2. Email: r.mcminds@thecnidaegritty.org
3. Describe the vulnerability
4. Allow time for a fix before disclosure

## 📜 Code of Conduct

### Our Pledge

We are committed to providing a welcoming and inclusive experience for everyone.

### Expected Behavior

- Be respectful and inclusive
- Accept constructive criticism gracefully
- Focus on what's best for the community
- Show empathy toward others

### Unacceptable Behavior

- Harassment or discrimination
- Trolling or insulting comments
- Personal or political attacks
- Publishing others' private information

## 📚 Additional Resources

### Jekyll Resources
- [Jekyll Documentation](https://jekyllrb.com/docs/)
- [Jekyll Themes](https://jekyllrb.com/docs/themes/)
- [Liquid Templating](https://shopify.github.io/liquid/)

### GitHub Resources
- [GitHub Pages Documentation](https://docs.github.com/en/pages)
- [Git Submodules](https://git-scm.com/book/en/v2/Git-Tools-Submodules)
- [GitHub Actions](https://docs.github.com/en/actions)

### Project-Specific
- [ARCHITECTURE.md](ARCHITECTURE.md): Technical architecture details
- [README.md](README.md): Project overview
- [multilingual_journal_club](https://github.com/rmcminds/multilingual_journal_club): Content repository

## ❓ Getting Help

### Questions?

- **General questions**: Open a discussion in GitHub Discussions
- **Technical issues**: Open an issue
- **Private matters**: Email r.mcminds@thecnidaegritty.org

### Response Time

- Issues: Usually within 1-2 weeks
- Pull requests: Review within 1-2 weeks
- Security issues: Within 48 hours

## 🙏 Thank You!

Your contributions make The Cnidae Gritty better for everyone. Whether you're fixing a typo, writing a blog post, or adding a major feature, we appreciate your time and effort!

---

*Happy contributing!* 🦑
