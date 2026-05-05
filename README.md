# Daniel B. Faizakoff P.C. — Website

A modern, maintenance-free static website for the firm.

## What's in this folder

```
legalesq-site/
├── index.html          Homepage
├── style.css           Shared stylesheet for all pages
├── practice-areas.html
├── practice-areas/     Estate Planning, Probate, Business detail pages
├── about.html
├── about/              3 attorney profile pages
├── contact.html        Contact page with Web3Forms
├── blog.html           Blog listing
├── blog/               26 individual blog post pages
├── terms.html
├── privacy.html
├── disclaimers.html
├── assets/             Images (logo, badges, attorney photos)
└── README.md           This file
```

## Before you deploy

Two items need your action before the site goes live:

### 1. Set up the Contact Form (Web3Forms)

The contact form in `contact.html` is pre-wired to Web3Forms. You need to replace the placeholder access key with your real one.

1. Go to https://web3forms.com
2. Enter your email (e.g., `dbf@legalesq.com`) — they'll email you an access key
3. Copy the access key from the email
4. Open `contact.html` in a text editor
5. Find this line (near the form):
   ```
   <input type="hidden" name="access_key" value="YOUR_ACCESS_KEY_HERE">
   ```
6. Replace `YOUR_ACCESS_KEY_HERE` with your access key
7. Save the file

Form submissions will now arrive at the email you signed up with, with the subject line "New Inquiry from legalesq.com".

### 2. Set up the Google Reviews Widget (Elfsight)

The Reviews section on the homepage has a clearly-marked paste area for an Elfsight widget embed code.

1. Go to https://elfsight.com/google-reviews-widget/
2. Click **"Create Widget"** or **"Start for Free"**
3. Sign up (email only — no credit card required on free tier)
4. When prompted, **connect your Google Business Profile** — this is how the widget pulls your real reviews
5. Search for your firm's listing and select it
6. Customize the look. Suggested settings for this site:
   - **Layout:** Grid (3 columns) or Slider
   - **Show:** Rating stars, review count, review text, reviewer photo
   - **Theme:** Light background to match the site
   - **Width:** 100% (full-width inside the card)
7. Once you're happy, click **"Add to Website"**. Elfsight will give you a snippet that looks like:
   ```html
   <script src="https://static.elfsight.com/platform/platform.js" async></script>
   <div class="elfsight-app-xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"></div>
   ```
8. Open `index.html` in a text editor
9. Find this block:
   ```html
   <!-- ============================================================ -->
   <!-- PASTE ELFSIGHT EMBED CODE BETWEEN THESE COMMENT MARKERS -->
   <!-- (see instructions in README.md) -->
   <!-- ============================================================ -->

   <div class="reviews-placeholder">
     ...
   </div>

   <!-- ============================================================ -->
   <!-- END ELFSIGHT EMBED AREA -->
   <!-- ============================================================ -->
   ```
10. **Delete everything between the two marker blocks** (including the `<div class="reviews-placeholder">` and its content), and paste your Elfsight snippet in its place. The result should look like:
    ```html
    <!-- ============================================================ -->
    <!-- PASTE ELFSIGHT EMBED CODE BETWEEN THESE COMMENT MARKERS -->
    <!-- ============================================================ -->

    <script src="https://static.elfsight.com/platform/platform.js" async></script>
    <div class="elfsight-app-xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"></div>

    <!-- ============================================================ -->
    <!-- END ELFSIGHT EMBED AREA -->
    <!-- ============================================================ -->
    ```
11. Save the file.

That's it. The reviews now load automatically on page view from any host.

**Free tier limit:** 200 widget views per month. If your traffic exceeds that, Elfsight shows an upgrade prompt. Their paid tier is ~$6/month.

### 3. Set up Google Analytics 4

Every page is pre-wired with a Google Analytics tracking snippet, but the snippet uses a placeholder Measurement ID (`G-XXXXXXXXXX`). Replace it with your real one:

1. Go to https://analytics.google.com and sign in with the Google account that owns (or will own) the firm's analytics
2. Click **"Start measuring"** if it's a new account, or **Admin → Create → Property** if you already have GA
3. Set property name (e.g., "Legalesq.com"), reporting timezone (Eastern Time), currency (USD)
4. Choose business size and objectives — pick whatever fits
5. Set up a **Web data stream**:
   - Website URL: `https://legalesq.com`
   - Stream name: `Legalesq Website`
6. After creating the stream, GA shows you a **Measurement ID** that looks like `G-XXXXXXXXXX` (with real letters/numbers instead of X's). Copy it.
7. **Search-and-replace in every HTML file** — the placeholder `G-XXXXXXXXXX` appears twice in each file's `<head>`. Easiest method:
   - **VS Code / Sublime / Notepad++:** Use Find & Replace Across Files (Ctrl+Shift+H or Cmd+Shift+H)
     - Find: `G-XXXXXXXXXX`
     - Replace: `G-YOUR-REAL-ID`
     - Apply to all files
   - **Mac Terminal:** From inside the `legalesq-site` folder, run:
     ```
     find . -name "*.html" -exec sed -i '' 's/G-XXXXXXXXXX/G-YOUR-REAL-ID/g' {} +
     ```
   - **Windows PowerShell:** From inside the `legalesq-site` folder, run:
     ```
     Get-ChildItem -Recurse -Filter "*.html" | ForEach-Object { (Get-Content $_.FullName) -replace 'G-XXXXXXXXXX', 'G-YOUR-REAL-ID' | Set-Content $_.FullName }
     ```
8. Re-upload to your host. Tracking starts on the next page view.
9. **Verify it's working:** Open your live site in one browser tab. In another tab, open Google Analytics → Reports → Realtime. You should see yourself listed as 1 active user within ~30 seconds.

**Privacy note:** The snippet I added includes `anonymize_ip: true` which truncates visitor IP addresses before storage — generally considered a privacy best practice and helpful for GDPR/CCPA compliance.

### 4. Set up the Newsletter Form (Mailchimp)

The footer of every page has a "Sign up for Legally Speaking" form pre-wired for Mailchimp. You need to replace the placeholder form action URL with your real one.

1. Sign in to Mailchimp at https://login.mailchimp.com
2. Go to **Audience → Signup Forms → Embedded Forms** (top right "Audience" menu, then sub-menu)
3. Pick the audience you want to add subscribers to
4. Choose any layout (we don't use Mailchimp's HTML — we just need the form action URL)
5. Look at the generated `<form action="...">` line in their code preview. The URL looks like:
   ```
   https://legalesq.us17.list-manage.com/subscribe/post?u=abc123&id=def456
   ```
   Note the `u=` value (your Mailchimp user ID) and `id=` value (your audience ID).
6. Also note the hidden `b_xxxxxxxxxx_yyyy` field name in their snippet — this is the anti-spam honeypot for your specific list.
7. **Now update your site files.** In every HTML file (40 files):
   - Find: `YOUR_MAILCHIMP_FORM_ACTION`
   - Replace with: the full URL from step 5
   - Find: `name="b_PLACEHOLDER"`
   - Replace with: `name="b_xxxxxxxxxx_yyyy"` (the actual hidden field name from step 6)

   Easiest way (Mac Terminal, from inside legalesq-site folder):
   ```
   find . -name "*.html" -exec sed -i '' \
     -e 's|YOUR_MAILCHIMP_FORM_ACTION|https://YOUR-FULL-URL-HERE|g' \
     -e 's|name="b_PLACEHOLDER"|name="b_REAL-VALUE-HERE"|g' \
     {} +
   ```
   Or use Find & Replace Across Files in VS Code / Sublime / Notepad++.
8. Re-upload to your host. Subscribers added from the footer form will now appear in your Mailchimp audience.

**Verify it works:** open the live site, scroll to the footer, enter a test email, click Subscribe. You should be redirected to Mailchimp's confirmation page, and the email should appear in your audience within a minute.

**To customize the welcome email:** in Mailchimp, go to **Audience → Settings → Email and List Settings** to edit the confirmation email and double opt-in flow.

---

## How to deploy it

Choose **one** of the following free hosts. All three support custom domains like legalesq.com.

### Option 1: Cloudflare Pages (recommended)

1. Go to https://pages.cloudflare.com/ and sign up for a free account
2. Click **"Create a project"** → **"Upload assets"**
3. Drag the entire `legalesq-site` folder onto the upload area
4. Name it (e.g., `faizakoff`) and click **"Deploy site"**
5. Within ~30 seconds, your site is live at `faizakoff.pages.dev`
6. To use `legalesq.com`: go to **Custom domains** → **Set up a custom domain** → enter `legalesq.com` and follow their DNS instructions

### Option 2: Netlify Drop (fastest, no signup)

1. Go to https://app.netlify.com/drop
2. Drag the unzipped `legalesq-site` folder onto the drop zone
3. Instant live URL like `https://xyz.netlify.app`
4. To connect your custom domain later, sign up for a free account

### Option 3: GitHub Pages

1. Create a free account at https://github.com
2. Create a new repository
3. Upload all files from this folder to the repo
4. **Settings** → **Pages** → pick `main` branch → save
5. Live at `yourusername.github.io/reponame` in ~1 minute

---

## How to edit the site

### Change text on any page
Open the file in any text editor. Find the text, edit it, save. Re-upload to your host (or if you're using GitHub Pages, commit and push).

### Change colors across the whole site
Open `style.css`. Near the top, find:
```css
:root {
  --red: #C8272C;
  --blue: #1D3767;
  --green: #5A9B3E;
}
```
Change those hex values. Everything across the site updates automatically.

### Add a new blog post
1. Copy an existing post file in `blog/` (e.g., `blog/medical-aid-in-dying-act.html`) to a new filename
2. Edit the new file — change the title in three places (the `<title>` tag, the hero eyebrow/heading, and the body)
3. Open `blog.html` and add a new card at the top of the grid, copying the pattern of the existing cards
4. Re-upload.

### Replace a photo
Just replace the file in `assets/` with a new file of the same name.

---

## Backup

Keep a copy of this folder safe (Dropbox, Google Drive, external drive). If anything ever happens to your host, you can redeploy to any of the three options above in minutes.
