# Trim Views Carpentry — deployment notes

## What to upload

Upload the **contents** of the project root to your host's web root
(`public_html`, `/`, `htdocs`, or a connected Git branch):

```
index.html            about.html            services.html
process.html          gallery.html          contact.html
terms.html            privacy-policy.html
assets/
  css/styles.css        ← the one shared stylesheet
  js/site.js          ← the one shared script
  fonts/              ← Garet webfonts (see note below)
images/               ← all photography, logo and favicons
```

**Do not upload `_source/`.** It holds working files only, including
`manifest.tsv`, whose third column contains client names.

## Paths

Every reference is a document-relative path with no leading slash
(`assets/css/styles.css`, `images/logo.png`, `about.html`). That means the site
works unchanged whether it is served from a domain root, a subfolder, a staging
subdomain, or opened straight off a local disk. Nothing needs rewriting at
deploy time.

The only absolute URLs are intentional third-party ones: Google Fonts, the
LeadConnector form embed, and the Facebook/Instagram profile links.

## Server-side requirements

None. This is pure static HTML, CSS, JS and images. It needs no PHP, no
Node runtime, no database, and no build step.

Suitable hosts include Netlify, Cloudflare Pages, GitHub Pages, Vercel,
Amazon S3 + CloudFront, or any ordinary cPanel/shared host.

## Forms

The contact page embeds a **LeadConnector / GoHighLevel** form
(`api.leadconnectorhq.com/widget/form/QBARiDpkkceV6YYbWsJw`) inside an iframe,
with its resize helper loaded from `link.msgsndr.com/js/form_embed.js`.

Submissions therefore go to the LeadConnector account, not to the web server.
No mail script, no backend, and no third-party form service is needed. Two
consequences worth knowing:

- If the LeadConnector sub-account is ever closed or the form ID changes, the
  contact page silently shows an empty iframe. Worth a quarterly check.
- Form styling and field validation are controlled inside LeadConnector, not
  in `styles.css`.

## Hosting requirements and limits

- **HTTPS** — required. The form embed and Google Fonts are HTTPS-only, and
  browsers will flag mixed content.
- **Fonts** — `styles.css` declares `@font-face` for *Garet* and expects the
  files in `assets/fonts/`. Those files were not part of the supplied project,
  so headings currently fall back to DM Sans / system sans-serif. Drop the
  licensed `Garet-Book.woff2` and `Garet-Bold.woff2` into `assets/fonts/` and
  headings pick it up with no code change.
- **Caching** — set a long `Cache-Control` on `assets/` and `images/`, and a
  short one on the `.html` files. On Netlify/Cloudflare this is the default.
- **Compression** — enable gzip or brotli for `.html`, `.css` and `.js`.
- **Clean URLs** — optional. If you want `/about` instead of `/about.html`,
  most hosts can do this with a redirect rule; keep the `.html` files as the
  canonical target and let the host handle it, so internal links stay valid.
- **404 page** — not currently present. Worth adding before launch.
