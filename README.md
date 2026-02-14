# The Cnidae Gritty

[![Jekyll Site CI](https://img.shields.io/badge/built%20with-Jekyll-red.svg)](https://jekyllrb.com/)
[![GitHub Pages](https://img.shields.io/badge/hosted%20on-GitHub%20Pages-blue.svg)](https://pages.github.com/)

**The Cnidae Gritty** is a Jekyll-based blog and web platform dedicated to communicating biology, particularly cnidarian research, in an informal and accessible way. This site serves as the web interface and hosting platform for various interactive tools and multilingual scientific content.

🌐 **Live Site**: [https://thecnidaegritty.org](https://thecnidaegritty.org)

## 🎯 Purpose

This repository serves as the **web development and hosting layer** for:
- Jekyll-based blog posts and articles
- Interactive R/Shiny applications (iNatle, photo annotator)
- Multilingual scientific content from the [multilingual_journal_club](https://github.com/rmcminds/multilingual_journal_club) repository
- Static pages and documentation

## 🏗️ Architecture

The Cnidae Gritty follows a modular architecture that separates concerns:

1. **Web Layer** (this repository): Jekyll configuration, layouts, themes, and web hosting
2. **Content Layer**: Integrated via git submodules and GitHub Actions from external repositories:
   - [multilingual_journal_club](https://github.com/rmcminds/multilingual_journal_club): Language-focused scientific content
   - [iNatle](https://github.com/rmcminds/iNatle): Interactive R/Shiny application

For detailed architectural information, see [ARCHITECTURE.md](ARCHITECTURE.md).

## 📁 Repository Structure

```
thecnidaegritty/
├── _config.yml              # Jekyll site configuration
├── _data/                   # Site data files (authors, etc.)
├── _includes/               # Reusable HTML components
├── _layouts/                # Page layouts (home, post, etc.)
├── _pages/                  # Static pages (about, author profiles)
├── _posts/                  # Blog posts (standard Jekyll posts)
├── _multilingual_posts/     # Posts synced from multilingual_journal_club
├── assets/                  # CSS, JavaScript, images
├── iNatle_raw/              # Deployed iNatle Shiny app
├── photo_annotator/         # Photo annotation Shiny app
├── scripts/                 # Build and integration scripts
├── .github/workflows/       # GitHub Actions for automated sync
├── ARCHITECTURE.md          # Detailed architecture documentation
├── CONTRIBUTING.md          # Contributor guidelines
└── README.md               # This file
```

## 🚀 Quick Start

### Prerequisites
- Ruby 2.7+ and Bundler
- Git

### Local Development

1. **Clone the repository**:
   ```bash
   git clone https://github.com/rmcminds/thecnidaegritty.git
   cd thecnidaegritty
   ```

2. **Install dependencies**:
   ```bash
   bundle install
   ```

3. **Serve the site locally**:
   ```bash
   bundle exec jekyll serve
   ```

4. **Open in browser**: Navigate to `http://localhost:4000`

### Working with Submodules

If the multilingual_journal_club submodule is initialized:

```bash
# Initialize and update submodules
git submodule update --init --recursive

# Pull latest changes from submodule
git submodule update --remote
```

## 🔄 Integration with multilingual_journal_club

Content from the [multilingual_journal_club](https://github.com/rmcminds/multilingual_journal_club) repository is automatically synchronized to this blog through:

1. **Git Submodule**: Links to the multilingual_journal_club repository
2. **GitHub Actions**: Automated workflow syncs content on schedule and manual trigger
3. **Jekyll Collections**: Content appears in the `_multilingual_posts` collection

For detailed workflow information, see [CONTRIBUTING.md](CONTRIBUTING.md).

## 🛠️ Technologies

- **Jekyll**: Static site generator
- **GitHub Pages**: Hosting platform
- **GitHub Actions**: CI/CD and content synchronization
- **R/Shiny**: Interactive applications
- **Shinylive**: Browser-based R/Shiny deployment

## 📚 Documentation

- [ARCHITECTURE.md](ARCHITECTURE.md): Detailed technical architecture
- [CONTRIBUTING.md](CONTRIBUTING.md): Contribution guidelines and workflows
- [About Page](https://thecnidaegritty.org/about/): Site mission and goals

## 👥 Contributing

We welcome contributions! Please see [CONTRIBUTING.md](CONTRIBUTING.md) for:
- How to contribute blog posts
- Working with Jekyll and the blog platform
- Integrating content from multilingual_journal_club
- Development workflow and best practices

**Note**: If you're contributing multilingual scientific content, please contribute directly to the [multilingual_journal_club](https://github.com/rmcminds/multilingual_journal_club) repository. Changes there will be automatically synced to this blog.

## 📝 License

This repository contains the Jekyll blog framework and web development files. Content from integrated repositories (multilingual_journal_club, iNatle) retains its original licensing.

## 🐛 Issues and Support

For issues related to:
- **Blog functionality, Jekyll, or web interface**: Open an issue in this repository
- **Multilingual content or translations**: Open an issue in [multilingual_journal_club](https://github.com/rmcminds/multilingual_journal_club)
- **iNatle app functionality**: Open an issue in [iNatle](https://github.com/rmcminds/iNatle)

## 📧 Contact

- **Author**: Ryan McMinds
- **Email**: r.mcminds@thecnidaegritty.org
- **Twitter**: [@thecnidaegritty](https://twitter.com/thecnidaegritty)
- **Mastodon**: [@thecnidaegritty@sciencemastodon.com](https://sciencemastodon.com/@thecnidaegritty)

---

*Getting into the cnidae gritty details of biology* 🦑
