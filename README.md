# Galleon8 Personal Website

![Galleon8 social preview: Tech Writing, UX Writing, and Docs Engineering](static/galleon8-social-preview-v2.png)

**Galleon8** is a personal professional website and publishing space for technical writing, UX writing, docs engineering, and reflective writing about documentation, communication, and the craft of technical writing.

Visit the live site: [galleon8.com](https://galleon8.com/)

Related professional showcase: [Galleon8 Docs](https://docs.galleon8.com/) is a separate Fern-based developer-documentation project that complements this site.

## Project Overview

Galleon8 is designed as a professional home rather than a generic blog. It brings together long-form writing, career context, technical communication interests, and public professional links in a deliberately maintained static site.

The site presents work and perspective across technical writing, UX writing, docs-as-code, developer experience, content strategy, and documentation engineering. Its current content includes a posts index, long-form personal and professional essays, an About page, a Resume page, and a downloadable CV.

Hugo is the implementation layer. The project uses Hugo and the Terminal theme as a foundation, then customizes the content structure, layout overrides, visual identity, metadata, navigation, responsive behavior, and four-theme design system.

## Website Highlights

- Professional publishing space centered on technical writing, UX writing, docs engineering, and the perspective behind the work.
- Homepage and posts index that introduce the editorial direction and surface published essays.
- Individual posts with publication dates, descriptions, reading-time metadata, and optional tags.
- About and Resume pages with long-form professional narrative, experience, tools, industry background, and a downloadable CV.
- Responsive navigation with links to About, Posts, Resume, Docs, and LinkedIn.
- Custom four-theme picker built on top of the Terminal theme's CSS-variable system.
- Open Graph and Twitter Card metadata with a site-level social-preview image.

## Content And Information Architecture

The public site is intentionally compact:

- **Homepage**: renders the posts section at `/`, using `content/posts/_index.md` as the introductory editorial landing content.
- **Posts index**: available at `/posts/`, listing published posts with dates, descriptions, reading time, and read-more links.
- **Individual posts**: stored in `content/posts/` and used for long-form essays about technical writing, personal context, tools, technology, and professional development.
- **About**: `content/about.md` explains the personal and professional path behind the site.
- **Resume**: `content/resume.md` presents professional background, tools, industry experience, and links to the CV PDF in `static/files/`.
- **Professional links**: the main navigation includes Docs and verified LinkedIn links from `hugo.toml`.
- **Feeds and metadata**: Hugo generates RSS output, while custom head templates define canonical, Open Graph, Twitter Card, favicon, manifest, and robots metadata.

## Visual Identity And Theming

The site keeps Terminal's retro, monospaced foundation while adding a custom Galleon8 palette system. Theme colors are implemented as CSS variables; the default palette comes from the Terminal theme, and the additional palettes live in `static/style.css`.

| Internal ID | Reader-facing name | Background | Foreground | Accent |
| --- | --- | --- | --- | --- |
| `default` | `dark & yellow` | `#1a170f` | `#eceae5` | `#eec35e` |
| `dark-blue` | `dark & blue` | `#0e1923` | `#d6e8ee` | `#5accf0` |
| `light-paper-green` | `light & green` | `#fff4f2` | `#424140` | `#008000` |
| `light-paper-purple` | `light & purple` | `#fff5eb` | `#45372b` | `#990bda` |

Theme behavior is custom:

- `layouts/partials/theme-picker.html` renders the theme control in desktop and mobile navigation.
- `static/js/theme-picker.js` cycles through the four theme IDs and stores the selected value in `localStorage` under `g8-blog-theme`.
- Non-default themes are applied through `html[data-theme="..."]`.
- `layouts/partials/extended_head.html` applies a saved non-default theme before the deferred picker script runs, reducing theme flash during page load.
- `static/style.css` also defines swatch colors, theme-picker styling, accent hover states, focus outlines, blockquote styling, list markers, footer layout, and responsive navigation adjustments.

The palettes are unified by the same three core tokens: `--background`, `--foreground`, and `--accent`. Terminal's base components, code styling, links, buttons, image borders, focus states, and syntax highlighting reference those variables, so typography and component styling adapt as the selected palette changes.

## Technical Implementation

- **Static site generator**: Hugo, configured in `hugo.toml`.
- **Theme foundation**: [Terminal](https://github.com/panr/hugo-theme-terminal), included as a Git submodule in `themes/terminal`.
- **Content model**: Markdown files with TOML front matter under `content/`.
- **Layout customization**: project-level overrides in `layouts/` for the homepage, posts list, single pages, metadata, footer, navigation, theme picker, and Markdown rendering.
- **Styling**: upstream Terminal CSS plus custom overrides in `static/style.css`.
- **JavaScript**: Terminal's bundled menu/code scripts plus custom theme switching in `static/js/theme-picker.js`.
- **Assets**: favicons and manifest files in `static/`, source favicon images in `assets/`, social preview at `static/galleon8-social-preview-v2.png`, and page imagery under `content/posts/` and `static/images/`.
- **Deployment**: GitHub Pages through the workflow in `.github/workflows/hugo.yml`.

Node.js is not required for normal local development of this repository. The package files are part of the upstream Terminal theme, not root project tooling.

## Repository Structure

```text
.
|-- .github/workflows/      # GitHub Pages deployment workflow
|-- archetypes/             # Starter front matter for new Hugo content
|-- assets/                 # Source assets, including favicon source images
|-- content/                # Authored pages, posts, and post-local media
|-- layouts/                # Project-level Hugo template and partial overrides
|-- static/                 # Published static assets, custom CSS, JS, favicons, CV, and social preview
|-- themes/terminal/        # Terminal theme Git submodule
|-- hugo.toml               # Site configuration, metadata, navigation, and Hugo settings
`-- README.md               # Project documentation
```

Generated output and caches such as `public/`, `resources/_gen/`, and `.hugo_build.lock` are ignored or build-managed and are not part of the maintained source.

## Prerequisites And Local Development

Use Hugo Extended. The deployment workflow installs Hugo Extended `0.161.1`, which is also the locally verified version for this repository. The upstream Terminal theme declares Hugo Extended `0.90.x` as its minimum.

Clone the repository and initialize the theme submodule:

```bash
git clone https://github.com/galleon8/g8-blog.git
cd g8-blog
git submodule update --init --recursive
```

Run the local development server:

```bash
hugo server
```

Include draft content while previewing:

```bash
hugo server -D
```

Hugo serves the site locally at `http://localhost:1313/` by default.

## Content Authoring Workflow

Authored content lives in `content/`:

- `content/posts/_index.md` introduces the posts section and is also used by the homepage.
- `content/posts/*.md` contains individual published posts.
- `content/about.md` and `content/resume.md` are standalone pages.
- Images used by posts can live beside post content in `content/posts/`; shared static images live in `static/images/`.
- The downloadable CV is stored in `static/files/`.

Post front matter follows `archetypes/posts.md`:

```toml
+++
title = "Example Post"
date = "2026-06-16T00:00:00+03:00"
description = "Short summary for lists and metadata."
tags = []
keywords = []
showFullContent = false
readingTime = true
+++
```

Current post conventions:

- `description` appears in post lists and social metadata.
- `date` controls publication metadata.
- `readingTime = true` enables reading-time and word-count display unless hidden.
- `tags` are supported by the templates when present.
- `keywords` can feed page metadata.
- `showFullContent = false` keeps list pages summary-oriented.

For new posts, create a Markdown file under `content/posts/` and follow the post archetype and existing published posts. The Hugo content-creation command is available in Hugo, but this README avoids presenting it as a verified project command because running it creates a new content file.

## Production Build And Deployment

Build the production site locally with:

```bash
hugo --gc --minify
```

The build outputs static files to `public/`.

Deployment is handled by `.github/workflows/hugo.yml`:

- Runs on pushes to `main` and on manual `workflow_dispatch`.
- Checks out the repository with recursive submodules.
- Installs Hugo Extended `0.161.1`.
- Builds with `hugo --gc --minify` and the GitHub Pages base URL.
- Uploads `./public` as a GitHub Pages artifact.
- Deploys through `actions/deploy-pages`.

The repository does not include a root-level `CNAME` file. The site configuration sets:

```toml
baseURL = "https://galleon8.com/"
```

GitHub Pages custom-domain configuration is external to the repository unless represented elsewhere in the project.

## Maintenance Reference

- **Site title, base URL, metadata, pagination, and Hugo settings**: `hugo.toml`
- **Main navigation, Docs link, and LinkedIn link**: `hugo.toml`
- **Homepage and posts index behavior**: `layouts/index.html`, `layouts/partials/posts-landing.html`, `content/posts/_index.md`
- **Single-page rendering and post metadata**: `layouts/_default/single.html`
- **Theme colors and responsive style overrides**: `static/style.css`
- **Theme switching behavior**: `static/js/theme-picker.js`
- **Early saved-theme application**: `layouts/partials/extended_head.html`
- **Theme-picker markup**: `layouts/partials/theme-picker.html`
- **Navigation overrides**: `layouts/partials/menu.html`, `layouts/partials/mobile-menu.html`
- **Metadata, favicons, RSS, and social previews**: `layouts/partials/head.html`
- **Footer copy**: `layouts/partials/footer.html`
- **Markdown link and image rendering**: `layouts/_markup/render-link.html`, `layouts/_markup/render-image.html`
- **Favicons and manifest**: `static/favicon.ico`, `static/favicon-*.png`, `static/apple-touch-icon.png`, `static/android-chrome-*.png`, `static/site.webmanifest`
- **Social-preview image**: `static/galleon8-social-preview-v2.png`
- **CV PDF**: `static/files/`
- **Theme attribution and license**: `themes/terminal/theme.toml`, `themes/terminal/LICENSE.md`
- **Deployment workflow**: `.github/workflows/hugo.yml`

## Quality And Verification

Verified project checks:

```bash
hugo --gc --minify
```

This confirms the Hugo build succeeds with the current content, templates, theme submodule, and static assets. Use `hugo server` for local responsive and theme-picker review before publishing content changes.

No root-level automated linting, formatting, accessibility, image-optimization, or broken-link checking tooling is currently configured.

## Scope, Attribution, And Ownership

This site is based on the [Terminal Hugo theme](https://github.com/panr/hugo-theme-terminal) by panr/Radoslaw Koziel. The theme is included as a Git submodule at `themes/terminal` and is licensed under MIT; preserve the upstream license and attribution in `themes/terminal/LICENSE.md`.

Inherited from Hugo and Terminal:

- Static-site generation, Markdown rendering, taxonomies, feeds, pagination, and Hugo Pipes behavior.
- Terminal's base typography, layout system, responsive navigation foundation, code styling, shortcodes, and CSS-variable approach.

Customized for Galleon8:

- Site positioning, editorial structure, authored content, professional metadata, footer copy, public navigation, favicons, social-preview image, CV asset, layout overrides, Markdown render hooks, theme-picker UI, custom CSS, custom JavaScript, and the four-theme palette system.

The website content, personal branding, Galleon8-specific visual assets, and professional positioning belong to the site owner unless otherwise noted. Upstream theme code remains governed by the Terminal theme license.

## Professional Links

Galleon8 is maintained as a professional publishing space focused on technical writing, UX writing, documentation engineering, developer documentation, content strategy, docs-as-code, AI-ready knowledge, and clear communication for complex systems.

- Website: [galleon8.com](https://galleon8.com/)
- LinkedIn: [Professional profile](https://www.linkedin.com/in/gene-danilov/)
- Documentation showcase: [Galleon8 Docs](https://docs.galleon8.com/)
