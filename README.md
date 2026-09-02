# Acai Frontend Starter

Acai is a lightweight, Gulp-powered frontend starter for building responsive HTML, CSS, and JavaScript projects. It combines a CDN-first technology stack with Sass, reusable HTML partials, live browser updates, asset optimization, and documentation pages that demonstrate how the system fits together.

The starter removes repetitive setup without hiding the underlying tools. Use the default stack for a fast start, or replace individual pieces as the project grows.

## What Is Included

- Gulp 5 development and production build workflow
- Bootstrap 5 through CDN integration
- Font Awesome through CDN integration
- jQuery through CDN integration
- Sass compilation with modular partials
- Reusable HTML partials with `gulp-file-include`
- BrowserSync development server and live reload
- HTML minification
- CSS minification and source maps
- JavaScript minification with Terser and source maps
- Image optimization with imagemin
- Automatic `robots.txt` copying
- Automatic sitemap generation from built HTML pages
- Shared metadata, Open Graph, Twitter card, favicon, font, and stylesheet partials
- Responsive typography using Sass `clamp()` values
- GitHub Pages-friendly static output

## Typography System

Acai includes four Google Fonts, each selected for a specific communication role. The fonts are loaded once through `src/partials/head/fonts.html` and exposed through Sass variables and generated utility classes in `src/scss/base/_typography.scss`.

| Font | Role | Strategy |
| --- | --- | --- |
| Space Grotesk | Brand and headings | Distinctive geometric character for titles, navigation, and high-level identity |
| Merriweather | Body copy | Serif letterforms that support comfortable reading in longer paragraphs |
| Montserrat | Examples and interface demonstrations | Clean, familiar sans-serif for component examples and alternate UI treatments |
| Barlow | Utility and supporting text | Practical sans-serif for labels, metadata, and compact utility content |

### Font Strategy

The system separates display, reading, example, and utility typography instead of assigning one font to every element:

- Headings use `$brand-font`, currently Space Grotesk, with a bold weight and a tight `1.15` line height.
- Paragraphs use `$body-font`, currently Merriweather, with a readable `1.7` line height.
- `.example-text` uses Montserrat for examples and component demonstrations.
- `.utility-text` uses Barlow for supporting UI and utility content.
- `$primary-font` and `$secondary-font` remain available as project-level aliases for Montserrat and Barlow.

This role-based approach creates hierarchy while keeping font decisions centralized. To change the visual direction of a project, update the font variables rather than rewriting every component.

All four families include regular and italic variants. Montserrat and Space Grotesk expose numeric weight classes from `100` through `900`; Barlow and Merriweather expose named classes such as `.barlow-bold` and `.merriweather-regular`.

Example:

```html
<h2 class="space-grotesk-700">A clear display heading</h2>
<p class="merriweather-regular">Readable supporting content belongs to the body role.</p>
<span class="barlow-semibold-italic">Supporting metadata</span>
```

### Responsive Type

Heading and paragraph sizes use Sass `clamp()` values so type scales between mobile and desktop without a collection of breakpoint overrides. The system defines sizes from `h1` through `h7`, plus a responsive paragraph size, in one place.

```scss
$h1-font: clamp(3rem, 2.5385rem + 2.0513vw, 4rem);
$p-font: clamp(1rem, 0.8846rem + 0.5128vw, 1.25rem);
```

## Requirements

- Node.js `20.19.0` or newer
- npm

## Installation

Clone the repository and install its dependencies:

```bash
git clone https://github.com/crendon213wp/acai-framework
cd acai-framework
npm install
```

Start the development workflow:

```bash
gulp
```

This runs Gulp, builds the project into `dist/`, starts BrowserSync, and watches HTML, Sass, JavaScript, images, and `robots.txt` for changes.

## Build Commands

```bash
gulp           # Build, serve with BrowserSync, and watch for changes
npm run build  # Run the Gulp build workflow
```

The default Gulp task runs these stages in order:

1. Clean `dist/`
2. Expand HTML partials
3. Minify built HTML
4. Compile and minify Sass
5. Minify project JavaScript
6. Optimize images
7. Copy `robots.txt`
8. Generate a sitemap
9. Start BrowserSync and file watching

## Project Structure

```text
project/
├─ src/
│  ├─ *.html                 # Page entry points
│  ├─ partials/              # Shared page and documentation fragments
│  │  ├─ head.html
│  │  ├─ head/               # Metadata, fonts, social tags, and styles
│  │  ├─ navbar.html
│  │  ├─ footer.html
│  │  ├─ components/
│  │  ├─ docs/
│  │  └─ utilities/
│  ├─ scss/
│  │  ├─ application.scss    # Sass entry point
│  │  └─ base/
│  │     ├─ _index.scss
│  │     └─ _typography.scss
│  ├─ js/app.js
│  ├─ images/
│  └─ robots.txt
├─ dist/                     # Generated output, not source
├─ site.config.js            # Site and sitemap settings
├─ gulpfile.js
├─ package.json
├─ readme.md
└─ secure.md
```

The current starter pages are `index.html`, `components.html`, `documentation.html`, `github.html`, and `utilities.html`. Files inside `src/partials/` are excluded from direct output and are included into those entry points during the build.

## HTML Partials

Use the shared head partial on each page so metadata and assets stay consistent:

```html
@@include('partials/head.html', {
    "title": "Home",
    "description": "A responsive frontend starter powered by Gulp and Sass.",
    "pageUrl": ""
})
```

Include shared navigation, footer, modal, documentation, or utility sections the same way:

```html
@@include('partials/navbar.html', { "active": "home" })
@@include('partials/footer.html')
```

## Sass Workflow

`src/scss/application.scss` is the Sass entry point:

```scss
@use 'base' as *;
```

Add shared tokens, typography roles, and base styles to the modular Sass files under `src/scss/base/`. Gulp compiles the entry point, writes the resulting CSS to `dist/css/`, minifies it, and writes a source map beside it.

Keep design decisions centralized in variables when possible. The font roles and responsive type scale live in `_typography.scss`, so a project can change its visual language without editing every page.

## Site Configuration

Update `site.config.js` with the real project values before deployment:

```javascript
module.exports = {
    site: {
        title: 'Project Title',
        url: 'https://example.com',
        author: 'Your Name',
        description: 'Project Description',
        language: 'en',
        locale: 'en_US'
    },

    seo: {
        changefreq: 'weekly',
        priority: 1.0
    }
};
```

The site URL is used by the sitemap task. The shared head partials provide page-level metadata and social markup for the HTML entry points.

## Deployment

Build the project and publish the generated `dist/` directory to a static host such as GitHub Pages. Before publishing:

- Replace the placeholder values in `site.config.js`.
- Update each page's title, description, and canonical URL data.
- Confirm the generated sitemap uses the production domain.
- Confirm image paths and external CDN dependencies are available in production.
- Review `secure.md` for project security guidance.

## Design Philosophy

Acai favors a small, understandable system over a large abstraction layer:

- Use Bootstrap for responsive layout and common components.
- Use Sass for project-owned styling and reusable design tokens.
- Use partials to keep page composition maintainable.
- Use role-based typography to create hierarchy and keep font changes cheap.
- Keep configuration centralized and build output generated.
- Automate repetitive optimization tasks while leaving source files readable.

## License

This project is licensed under the MIT License.

## Author

Cisco Rendon

Frontend Systems | Responsive Development | Workflow Architecture | Digital Operations

First started: 2026

Last updated: 2026
