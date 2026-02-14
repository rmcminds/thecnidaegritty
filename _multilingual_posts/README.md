# Multilingual Posts Directory

This directory contains multilingual scientific content automatically synchronized from the [multilingual_journal_club](https://github.com/rmcminds/multilingual_journal_club) repository.

## ⚠️ Important: Do Not Edit Directly

**Files in this directory are automatically synchronized and should NOT be edited directly.**

### To Add or Edit Multilingual Content:

1. Go to the [multilingual_journal_club repository](https://github.com/rmcminds/multilingual_journal_club)
2. Follow the contribution guidelines in that repository
3. Your changes will be automatically synced to this directory

### How Synchronization Works:

- **Schedule**: Daily at 6 AM UTC
- **Manual Trigger**: Via GitHub Actions workflow
- **Webhook**: (Future) Automatic on multilingual_journal_club updates

### Content Format:

All files in this directory follow Jekyll post format:
- Filename: `YYYY-MM-DD-title.md`
- Frontmatter includes:
  - `layout: post`
  - `title: "Post Title"`
  - `date: YYYY-MM-DD HH:MM:SS -0400`
  - `categories: category-name`
  - `author: author-id`
  - `source_repo: multilingual_journal_club` (added automatically)

### URL Structure:

Posts in this collection are accessible at:
```
https://thecnidaegritty.org/multilingual/YYYY/MM/DD/title/
```

### For More Information:

- [ARCHITECTURE.md](../ARCHITECTURE.md): Technical integration details
- [CONTRIBUTING.md](../CONTRIBUTING.md): Contribution guidelines
- [multilingual_journal_club](https://github.com/rmcminds/multilingual_journal_club): Source repository

---

*This directory is managed by automated synchronization. Manual changes will be overwritten.*
