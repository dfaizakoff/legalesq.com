# Daniel B. Faizakoff P.C. — Website

A modern, maintenance-free static website for the firm. Built as plain HTML/CSS so it deploys anywhere and stays editable by anyone with a text editor.

**Live at:** https://legalesq.com

---

## What's in this folder

```
legalesq.com/
├── index.html                     Homepage
├── about.html                     About the firm
├── attorneys.html                 Attorneys landing
├── practice-areas.html            Practice areas landing
├── blog.html                      Blog listing
├── contact.html                   Contact + Web3Forms-backed form
│
├── about/                         3 attorney profile pages
│   ├── daniel-faizakoff.html
│   ├── richard-wender.html
│   └── ripal-gajjar.html
│
├── practice-areas/                3 practice detail pages
│   ├── estate-planning.html
│   ├── probate.html
│   └── business.html
│
├── blog/                          26 individual blog post pages
│
├── terms.html                     Terms of use
├── privacy.html                   Privacy policy
├── disclaimers.html               Attorney advertising disclaimers
├── 404.html                       Custom 404 page
│
├── style.css                      Shared stylesheet for every page
├── sitemap.xml                    For search engines
├── robots.txt                     For search engines
├── site.webmanifest               PWA / favicon manifest
│
├── assets/                        Images (logo, lockup, badges, attorney photos, favicons)
└── README.md                      This file
```

---

## What this site does well

- **Static HTML.** No build step, no Node, no framework. Open any file in a text editor, edit, save, re-upload.
- **Fast.** Pages weigh under 30KB each plus the shared 60KB stylesheet. Loads on slow connections.
- **Mobile-first.** Every page is fully responsive. A floating Call Now button appears on phones.
- **SEO-optimized.** Every page has unique `<title>`, meta description, Open Graph tags, Twitter Card tags, canonical URL, and JSON-LD structured data. Sitemap and robots.txt at the root.
- **Accessible.** Semantic HTML, alt text on every image, skip-to-content link, ARIA labels on icon-only buttons.
- **Privacy-conscious.** Google Analytics is configured with `anonymize_ip: true`.
- **Bidirectional internal linking.** Site pages link into the blog where keywords match, and blog posts deep-link into named anchor sections (`#why-plan-with-us`, `#special-situations`, etc.) on the practice pages. This is a deliberate SEO topic-cluster structure.

---

## Third-party integrations (already wired up)

The following services are live and configured. No action needed unless you want to change settings.

### Contact form — Web3Forms

Located in `contact.html`. Submissions are emailed to the access-key holder's address. The access key is baked into the form's hidden `access_key` field. To change the destination email or settings, log in at https://web3forms.com.

### Newsletter — Mailchimp (Legally Speaking)

Footer of every page. The form posts to a Mailchimp embedded-form endpoint with the firm's audience ID. To change which audience subscribers go to, edit the `<form action="...">` URL in the footer (it's repeated in every HTML file). Or to change once and propagate, search-and-replace across files:

```bash
# Mac / Linux
find . -name "*.html" -exec sed -i '' 's|legalesq.us5.list-manage.com/subscribe/post?u=OLD-ID|legalesq.us5.list-manage.com/subscribe/post?u=NEW-ID|g' {} +
```

### Reviews — GatherUp

Embedded on the homepage `index.html` in the Reviews section. The widget pulls live from Google Business Profile via GatherUp (`data-bid="139504"`). To change the widget, log in at https://app.gatherup.com.

### Google Analytics 4

Measurement ID `G-ZSM66VNQYK` is baked into every page's `<head>`. To change the property, search-and-replace across all HTML files.

### Calendly

The "Book a Consultation" buttons throughout the site open a Calendly popup pointing at `https://calendly.com/faizakoff/meeting-with-dan-faizakoff`. To change the destination, edit the Calendly URL in every HTML file (it appears multiple times per page).

---

## How to deploy

The site is deployed via **Cloudflare Pages**. The standard workflow:

1. Make your edits locally (or in GitHub's web editor).
2. Commit and push to the `main` branch of the repo.
3. Cloudflare Pages rebuilds and redeploys automatically within ~60 seconds.

The domain `legalesq.com` is on Cloudflare (nameservers `renan.ns.cloudflare.com` and `katja.ns.cloudflare.com`), proxied, with the zone under the **faizakoff@gmail.com** Cloudflare account. Traffic is served from Cloudflare's edge, which is also why the `_redirects` file works and why extensionless URLs such as `/wills-vs-revocable-trusts` resolve.

Note: this section previously said GitHub Pages. That was incorrect. GitHub Pages ignores `_redirects` entirely and does not serve extensionless URLs, so if it were true every one of the ~100 legacy WordPress redirects would be dead. Verified 2026.08.27 against live DNS, the Cloudflare-range A records, and a live fetch.

### Drag-and-drop method

When the firm's law-tech assistant sends an updated `.zip`:

1. Unzip locally.
2. Open the GitHub repo in a browser.
3. Drag the unzipped files/folders directly into the repo's root via GitHub's web UI (or use `git pull` / `git push` if you're comfortable on the command line).
4. Commit the change.
5. The live site updates in about a minute.

---

## How to edit the site

### Change body text on a page

Open the file in any text editor (VS Code, Sublime, Notepad++, or even GitHub's web editor). Search for the text you want to change, edit it, save, commit. Done.

### Change colors across the whole site

Open `style.css`. Near the top:

```css
:root {
  --cerulean: #11A0B5;       /* primary accent (links, button backgrounds) */
  --cerulean-dark: #0E8094;  /* hover state */
  --navy: #1D3767;           /* headings, dark backgrounds */
  --navy-dark: #142849;
  --navy-deep: #0D1D38;
  --text: #1A1A1A;
  --text-muted: #5A5A5A;
  /* ... */
}
```

Edit those hex values. Every component on the site updates automatically.

### Add a new blog post

1. Copy an existing post file in `blog/` (e.g., `blog/medical-aid-in-dying-act.html`) to a new filename. Use lowercase hyphenated slugs (e.g., `new-tax-rules-2026.html`).
2. In the new file, update:
   - `<title>` tag
   - `<meta name="description">`
   - The Open Graph and Twitter Card title/description meta tags
   - The canonical URL (`<link rel="canonical">`)
   - The `<h1>` headline and date in the hero
   - The article body
   - The "Related Reading" cards at the bottom (point to 2 sibling posts + 1 practice area)
   - The previous/next post navigation at the very bottom
   - The JSON-LD `BlogPosting` schema near the top of `<head>`
3. Open `blog.html` and add a new `<article class="blog-card">` block at the top of the grid, copying the pattern of the existing cards.
4. Add a new `<url>` entry to `sitemap.xml` with today's date.
5. Commit and push.

### Update an attorney photo

Replace the file in `assets/` keeping the same filename (`dan.jpg`, `wender.jpg`, `rip.jpg`). Optimize to under 200KB before uploading. Recommended dimensions: 800px square or 800×1000 portrait.

### Add a new practice area or attorney

Larger structural changes — best to send those back to the firm's law-tech assistant rather than hand-editing. New top-level pages need to be added to:

- The main `<nav>` menu (in every HTML file)
- The mobile menu (in every HTML file)
- The footer links (in every HTML file)
- `sitemap.xml`
- Cross-links from related pages
- JSON-LD structured data where applicable

That's a lot of consistent edits. The assistant can do it in one pass and ship a clean zip.

---

## SEO architecture (what's already in place)

Every page includes:

- Unique `<title>` and `<meta name="description">`
- `<link rel="canonical">` pointing at `https://legalesq.com/...`
- Open Graph tags (`og:type`, `og:url`, `og:title`, `og:description`, `og:image`, `og:site_name`)
- Twitter Card tags (`twitter:card`, `twitter:url`, `twitter:title`, `twitter:description`, `twitter:image`)
- JSON-LD structured data:
  - Homepage: `LegalService` + `LocalBusiness` schema with both office addresses
  - Attorney profiles: `Person` schema
  - Blog posts: `BlogPosting` schema with author, datePublished, dateModified

Blog posts cross-link to one another and to the practice-area pages on relevant keyword anchor text ("revocable trust," "beneficiary designations," "power of attorney," etc.) to build SEO topic clusters. The practice pages link back into the blog the same way.

### Named anchor IDs

The following section anchors exist for deep-linking from blog posts:

- `/practice-areas/estate-planning.html#why-plan-with-us`
- `/practice-areas/estate-planning.html#special-situations`
- `/practice-areas/probate.html#why-hire-us`
- `/practice-areas/business.html#why-founders-hire-us`

Use these anchors when adding new blog posts that should drive traffic into a specific section of a practice page.

---

## Backup

The GitHub repo is the source of truth. As long as the repo is intact, the site can be redeployed to any static host (Cloudflare Pages, Netlify, Vercel, S3, plain Apache) in minutes.

For additional safety, keep an occasional `.zip` of the repo on Dropbox or Google Drive.
