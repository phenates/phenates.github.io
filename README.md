# Portfolio - Stéphane Perfetti

Professional portfolio showcasing my skills, experience, and projects as a Systems, Network, and Security Administrator.

🌐 **Live site:** [stephaneperfetti.phenates.eu](https://stephaneperfetti.phenates.eu)

## About

Portfolio website built with Hugo and the Toha v4 theme, featuring:
- Professional experience and projects
- Technical skills (Systems, Networks, DevOps)
- Education and certifications
- Downloadable resume/CV


## Technologies Used

### Framework and Theme
- **[Hugo](https://gohugo.io/)** (Extended) - Static site generator
- **[Toha v4](https://github.com/hugo-toha/toha)** - Portfolio/blog theme via Hugo Modules
- **Hugo Modules** - Dependency management (no Git submodules)

### Assets and Dependencies
- **Node.js 20** - npm package management
- **Bootstrap** - CSS framework
- **Font Awesome** - Icon library
- **Mermaid** - Diagram rendering
- **KaTeX** - Mathematical expressions
- **Mulish** - Web font

### Deployment
- **GitHub Pages** - Hosting platform
- **GitHub Actions** - Automated CI/CD
- **Custom Domain** - stephaneperfetti.phenates.eu

## Project Structure

```
Portfolio/
├── .github/workflows/    # GitHub Actions (automated deployment)
├── content/              # Markdown content (projects, posts, notes)
│   ├── notes/
│   ├── posts/
│   └── projects/
├── data/fr/              # Site configuration and content
│   ├── author.yaml       # Author information
│   ├── site.yaml         # Site metadata
│   └── sections/         # Homepage sections
├── i18n/                 # Translations (FR/EN)
├── layouts/              # Custom templates
│   └── partials/
├── static/               # Static assets
│   ├── CNAME            # Custom domain configuration
│   ├── files/           # Resume and documents
│   ├── images/          # Site images
│   └── videos/          # Videos
├── go.mod               # Hugo module dependencies
├── hugo.yaml            # Main Hugo configuration
└── package.json         # npm dependencies
```


## Configuration

### Main Configuration Files

- **`hugo.yaml`** - Hugo configuration (baseURL, params, features)
- **`data/fr/site.yaml`** - Site metadata, custom menus, OpenGraph
- **`data/fr/author.yaml`** - Author information
- **`data/fr/sections/*.yaml`** - Homepage section configurations
- **`static/CNAME`** - Custom domain configuration

### Customization

#### Add a New Project
1. Create a markdown file in `content/projects/`
2. Add project metadata to `data/fr/sections/projects.yaml`
3. Add project images to `static/images/projects/`

#### Update Resume/CV
1. Place the PDF in `static/files/`
2. Update the link in `data/fr/site.yaml` (`customMenus` section)

#### Modify Homepage Sections
- Edit YAML files in `data/fr/sections/`
- Control visibility with `section.enable: true/false`
- Reorder sections with `section.weight` (lower weight = higher priority)


## Installation and Local Development

### Prerequisites
- **Hugo Extended** (latest version) - [Installation](https://gohugo.io/installation/)
- **Go** 1.20+ - [Installation](https://go.dev/doc/install)
- **Node.js** 20+ - [Installation](https://nodejs.org/)
- **Git** - [Installation](https://git-scm.com/)

### Installation

```bash
# Clone the repository
git clone https://github.com/phenates/phenates.github.io.git
cd phenates.github.io

# Update Hugo modules
hugo mod tidy

# Install npm dependencies
hugo mod npm pack
npm install
```

### Start Development Server

```bash
# Start server with drafts enabled
hugo server -D
# or
rm -r .\public\* -Force; hugo server -wD

# Site is accessible at http://localhost:1313
```

### Production Build

```bash
# Generate static site
hugo --minify

# Files are generated in ./public/
```

## Deployment

Deployment is **fully automated** via GitHub Actions. Every push to the `main` branch triggers a complete build and deployment pipeline.

### Deployment Flow

```
Push to main → Trigger Workflow → Build Site → Deploy to gh-pages → Live on Custom Domain
```

1. Push commits to the `main` branch
2. GitHub Actions workflow automatically triggers
3. Site is built with Hugo and minified
4. Generated files are deployed to `gh-pages` branch
5. GitHub Pages serves the site at https://stephaneperfetti.phenates.eu

### GitHub Actions Workflow Details

The deployment workflow is defined in `.github/workflows/deploy-site.yaml` and consists of the following steps:

#### 1. **Trigger Configuration**
```yaml
on:
  push:
    branches:
      - main
```
The workflow runs automatically when commits are pushed to the `main` branch.

#### 2. **Environment Setup**
- **Runner:** `ubuntu-latest` - GitHub-hosted Ubuntu runner
- **Permissions:** `contents: write` - Required to push to `gh-pages` branch

#### 3. **Workflow Steps**

**Step 1: Checkout Repository**
```yaml
- uses: actions/checkout@v4
```
- Action: `actions/checkout@v4`
- Purpose: Clone the repository with all files and history
- This provides the source files for the build process

**Step 2: Setup Hugo**
```yaml
- uses: peaceiris/actions-hugo@v3
  with:
    hugo-version: 'latest'
    extended: true
```
- Action: `peaceiris/actions-hugo@v3`
- Purpose: Install Hugo Extended (latest version)
- Extended version is required for SCSS/SASS processing

**Step 3: Update Hugo Modules**
```bash
hugo mod tidy
```
- Command: `hugo mod tidy`
- Purpose: Update and clean Hugo module dependencies
- Ensures the Toha theme and dependencies are up-to-date

**Step 4: Setup Node.js**
```yaml
- uses: actions/setup-node@v4
  with:
    node-version: 20
```
- Action: `actions/setup-node@v4`
- Purpose: Install Node.js version 20
- Required for npm dependencies (Bootstrap, Mermaid, KaTeX, etc.)

**Step 5: Install npm Dependencies**
```bash
hugo mod npm pack
npm install
```
- Commands:
  - `hugo mod npm pack` - Generate package.json from Hugo modules
  - `npm install` - Install all npm dependencies
- Purpose: Install theme assets (fonts, icons, libraries)

**Step 6: Build Site**
```bash
hugo --minify
```
- Command: `hugo --minify`
- Purpose: Generate static site with minification
- Output: `./public/` directory with optimized HTML, CSS, JS
- Minification reduces file sizes for faster loading

**Step 7: Deploy to GitHub Pages**
```yaml
- uses: peaceiris/actions-gh-pages@v4
  with:
    github_token: ${{ secrets.GITHUB_TOKEN }}
    publish_branch: gh-pages
    publish_dir: ./public
```
- Action: `peaceiris/actions-gh-pages@v4`
- Purpose: Deploy built site to `gh-pages` branch
- Authentication: Uses GitHub's automatic `GITHUB_TOKEN`
- Source: `./public/` directory (Hugo build output)
- Destination: `gh-pages` branch

#### 4. **GitHub Pages Configuration**

Once deployed to the `gh-pages` branch:
- GitHub Pages automatically serves the content
- Custom domain (`stephaneperfetti.phenates.eu`) is configured via `static/CNAME`
- The `CNAME` file is copied to the build output during Hugo build
- DNS must be configured at the domain provider to point to GitHub Pages


### Manual Deployment (if needed)

If you need to deploy manually:

```bash
# Build the site
hugo --minify

# Deploy to gh-pages branch (example using gh-pages npm package)
npm install -g gh-pages
gh-pages -d public -b gh-pages
```


## Troubleshooting

### Module Errors
```bash
hugo mod clean
hugo mod get -u
hugo mod verify
```

### Build Errors
- Verify Hugo Extended is installed: `hugo version`
- Update dependencies: `hugo mod tidy`
- Reinstall npm packages: `hugo mod npm pack && npm install`

### Images Not Displaying
- Verify `baseURL` in `hugo.yaml` is `https://phenates.github.io/`
- Images must be in `static/` and referenced with `/images/...`

## License

This portfolio uses the [Toha](https://github.com/hugo-toha/toha) theme under the MIT License.

Site content (text, personal images, CV) is © 2025 Stéphane Perfetti - All rights reserved.

