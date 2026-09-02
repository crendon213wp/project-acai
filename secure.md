# Security Policy

## Scope

Acai is a static frontend starter and build workflow. It compiles source files into `dist/`; it does not provide a backend, authentication system, database, API server, or secret-management service.

This policy describes the security baseline for the current `1.1.x` release line. Production projects built from Acai remain responsible for their hosting, domain, content, dependencies, and deployment configuration.

## Supported Versions

| Version | Supported |
| --- | --- |
| 1.1.x | Yes |
| Earlier 1.x | Best effort |

## Reporting a Vulnerability

Report security vulnerabilities, dependency issues, or configuration problems privately to the project maintainer before public disclosure. Include:

- A clear description of the issue
- Reproduction steps or a proof of concept, when safe to share
- Affected files, versions, or dependencies
- Potential impact and severity
- Any suggested mitigation

Do not include passwords, API keys, private tokens, or other live credentials in a report.

## Current Security Baseline

The repository is configured to exclude common local and generated data through `.gitignore`, including:

- `node_modules/`
- `dist/` and `build/`
- `.env`, `.env.local`, and environment-specific `.env` files
- npm, Yarn, and pnpm debug logs
- Coverage output, Sass cache, temporary files, and editor settings

The committed `package-lock.json` should be kept in sync with `package.json` so dependency installation can be reproduced and reviewed.

## Runtime and Dependencies

The current project requires Node.js `20.19.0` or newer. Build dependencies include Gulp, Sass, BrowserSync, HTML/CSS/JavaScript minifiers, image optimization, HTML partial processing, and sitemap generation.

Install from the lockfile in CI and deployment environments:

```bash
npm ci
```

Use `npm install` when adding or intentionally changing dependencies. Review the resulting lockfile diff before committing it.

Check dependencies regularly:

```bash
npm audit
npm outdated
```

Apply updates deliberately, run the build, and review the generated output after dependency changes. Do not use `npm update` as an unattended production deployment step.

## Secrets and Environment Files

Never commit passwords, API keys, access tokens, private certificates, or production credentials. Keep local values in ignored environment files when a separate application requires them:

```text
.env
.env.local
.env.development.local
.env.test.local
.env.production.local
```

This starter does not load environment variables itself. Do not assume that placing a secret in an environment file makes it safe to expose through frontend JavaScript: values sent to the browser are public.

## CDN Dependencies

The default pages load third-party assets from these pinned URLs:

- Bootstrap `5.3.8` from jsDelivr
- Popper `2.11.8` from jsDelivr
- jQuery `3.7.1` from cdnjs
- Font Awesome `7.0.1` from cdnjs
- Google Fonts: Barlow, Montserrat, Space Grotesk, and Merriweather

CDN resources are external runtime dependencies. Before production deployment:

- Keep versions explicitly pinned; avoid unversioned URLs.
- Verify the host, package, version, and requested asset before accepting an update.
- Consider self-hosting critical assets when availability, privacy, or supply-chain control matters.
- Add Subresource Integrity (`integrity` and `crossorigin`) where the CDN provides stable hashes and the asset can support SRI.
- Review privacy and consent requirements for Google Fonts and other third-party requests.

The current starter uses pinned CDN URLs but does not add SRI hashes. This is a known hardening opportunity for production projects.

## BrowserSync

BrowserSync is a local development server used by the Gulp workflow. It must not be used as a production web server or exposed to an untrusted network. Stop the development process before deployment and serve only the generated `dist/` directory through a properly configured production host.

## Build Output and Deployment

Treat `dist/` as generated output. Review it before publishing and confirm that it contains only intended public assets. Before a production deployment:

- Run the build from a clean, locked dependency install.
- Check for accidental credentials, debug data, source-only files, and development URLs.
- Confirm `site.config.js` uses the real HTTPS site URL before generating the sitemap.
- Confirm `robots.txt` and sitemap output are appropriate for the environment.
- Serve the site over HTTPS.
- Configure security headers at the hosting layer, including a suitable Content Security Policy, `frame-ancestors`, `Referrer-Policy`, and `X-Content-Type-Options`.
- Configure caching and access controls for the hosting platform.
- Review third-party scripts and analytics before enabling them.

Acai does not configure hosting headers, HTTPS certificates, DNS, access control, or server infrastructure.

## Frontend Security Boundaries

Because the output runs in a browser:

- Treat all user-provided content as untrusted and escape or sanitize it before inserting it into HTML.
- Do not place secrets in HTML, CSS, JavaScript, image metadata, or public configuration files.
- Avoid unsafe dynamic HTML insertion unless the input is trusted or sanitized.
- Keep third-party scripts to the minimum required by the project.
- Review external links and use appropriate `rel` attributes when opening new tabs.
- Validate accessibility and browser behavior as part of release review.

## Maintenance Checklist

Before each release:

1. Run `npm ci` from a clean checkout.
2. Run `npm audit` and review any findings.
3. Run the Gulp build and inspect `dist/`.
4. Search the generated output for credentials, private URLs, and development-only content.
5. Confirm CDN versions and third-party script changes.
6. Verify HTTPS, security headers, sitemap, `robots.txt`, and deployment settings.
7. Record any accepted risks and their planned remediation.

## Planned Improvements

Potential future improvements include:

- Automated dependency update and audit checks in CI
- SRI metadata for supported CDN assets
- Content Security Policy testing
- Lighthouse and accessibility checks in the release workflow
- Optional self-hosted assets for privacy and supply-chain control
- Reproducible production build artifacts

## Disclaimer

Acai is a frontend workflow foundation, not a complete application security solution. Developers and deployment owners are responsible for reviewing the generated site, dependencies, hosting configuration, third-party services, and production security requirements.
