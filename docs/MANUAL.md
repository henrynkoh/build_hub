# Build Hub Manual

Complete reference for the Construction Project Hub.

Repository: https://github.com/henrynkoh/build_hub

## 1. Overview

Build Hub is a static website with an optional local Python automation pipeline.

| Component | Details |
|---|---|
| Workflow | 15 construction steps |
| Materials | 69 material lines |
| Affiliate picks | 180 Amazon products |
| Shorts | 60 video definitions |
| Website stack | HTML, CSS, JavaScript |
| Pipeline | Python, ffmpeg, macOS `say`, YouTube Data API |

## 2. File Reference

### Website

| File | Purpose |
|---|---|
| `index.html` | Main dashboard |
| `step-01.html` to `step-15.html` | Individual workflow pages |
| `affiliate.html` | Affiliate campaign and CSV download |
| `shorts.html` | Browser preview and WebM rendering |
| `data.js` | Construction data and Amazon product picks |
| `app.js` | Navigation, tables, filters, and dynamic content |
| `styles.css` | Site styling and responsive rules |
| `shorts-data.js` | Browser Shorts definitions |

### Pipeline

| File | Purpose |
|---|---|
| `pipeline/run_pipeline.py` | Render MP4 files and upload to YouTube |
| `pipeline/shorts.json` | Pipeline Shorts definitions |
| `pipeline/out/` | Generated images, audio, and video output |
| `pipeline/state.json` | IDs of uploaded videos |
| `pipeline/client_secret.json` | Local Google OAuth credential, never commit |
| `pipeline/token.json` | Local authorization token, never commit |

## 3. Running the Website

Recommended:

```bash
cd build_hub
python3 -m http.server 8000
```

Open http://localhost:8000.

A local server avoids browser restrictions that can occur with `file://` URLs.

## 4. Website Operation

### Home page

The home page calculates totals from `data.js` and displays:

- Workflow sequence
- Material estimates
- Labor hours
- Searchable material table
- Supplier filters
- Tool and product cards

### Step pages

Each step page reads its step ID from the page's `data-id` attribute and uses `app.js` to render the correct materials, tools, affiliate picks, and adjacent-step navigation.

### Affiliate page

The Affiliate page displays all product picks and can export the campaign as CSV in the browser.

### Shorts Studio

The Studio reads `shorts-data.js`, draws vertical 1080 x 1920 frames on a canvas, and records WebM files through the browser's MediaRecorder API.

## 5. Updating Data

### Affiliate tag

Change `buildsmartpic-20` in:

1. `data.js`
2. `shorts-data.js`
3. `pipeline/shorts.json`

```bash
grep -rn "buildsmartpic-20" .
```

### Prices and products

Verify prices, ratings, review counts, availability, image URLs, and product identifiers before publishing. These values can change.

### Keeping Shorts synchronized

When updating video scripts or products, update both `shorts-data.js` and `pipeline/shorts.json`.

## 6. Python Pipeline

### Requirements

```bash
brew install ffmpeg
python3 -m venv pipeline/.venv
source pipeline/.venv/bin/activate
pip install Pillow requests google-api-python-client google-auth-oauthlib
```

### Command options

```text
--tag TAG          Required affiliate tag
--render           Render video files
--upload           Upload videos to YouTube
--per-day NUMBER   Maximum uploads for the run, default 6
--privacy STATUS   YouTube visibility, default private
--only ID          Process one Short ID
```

### Render examples

One Short:

```bash
python3 pipeline/run_pipeline.py --tag buildsmartpic-20 --render --only s01-m-1
```

All Shorts:

```bash
python3 pipeline/run_pipeline.py --tag buildsmartpic-20 --render
```

### Upload example

```bash
python3 pipeline/run_pipeline.py \
  --tag buildsmartpic-20 \
  --upload \
  --per-day 6 \
  --privacy private
```

The pipeline skips IDs already listed in `pipeline/state.json`.

## 7. Google and YouTube Setup

1. Create a Google Cloud project.
2. Enable YouTube Data API v3.
3. Configure the OAuth consent screen.
4. Create an OAuth Desktop App credential.
5. Download it as `client_secret.json`.
6. Save it at `pipeline/client_secret.json`.
7. Run one private upload and authorize in the browser.

Never publish the secret or token files.

## 8. Deployment

Build Hub requires no compilation. Publish the repository root to GitHub Pages, Netlify, Cloudflare Pages, Amazon S3, or another static host.

GitHub Pages setup:

1. Open repository **Settings**.
2. Select **Pages**.
3. Choose **Deploy from a branch**.
4. Select `main` and `/ (root)`.
5. Save and wait for the deployment URL.

## 9. Compliance

### Amazon Associates

- Use a clear affiliate disclosure near affiliate content.
- Do not guarantee price, availability, rating, or ranking.
- Follow Amazon's current brand, link, and price-display rules.

### Advertising and email

- Do not make unsupported claims.
- Send email only to opted-in contacts.
- Include unsubscribe and physical-address information where legally required.
- Follow each platform's advertising and community rules.

### Construction safety

- Treat all quantities and labor estimates as planning references.
- Confirm structural, electrical, plumbing, mechanical, and safety requirements with qualified professionals.
- Follow local codes, permits, manufacturer instructions, and PPE requirements.

## 10. Credential and Repository Safety

Recommended `.gitignore`:

```gitignore
pipeline/.venv/
pipeline/client_secret.json
pipeline/token.json
pipeline/state.json
pipeline/out/
.DS_Store
*.pickle
```

Before committing:

```bash
git status
git diff --cached
```

## 11. Troubleshooting

| Problem | Fix |
|---|---|
| Page has no styling | Confirm `styles.css` is present and serve from the repository root |
| Tables are empty | Confirm `data.js` loads before `app.js` |
| Shorts list is empty | Confirm `shorts-data.js` is present |
| Product images fail | Verify the remote image URLs and network access |
| Old affiliate tag remains | Search all files with `grep -rn` |
| `say` is missing | Run the pipeline on macOS or replace the narration engine |
| `ffmpeg` is missing | Run `brew install ffmpeg` |
| Python module is missing | Activate the virtual environment and install requirements |
| OAuth access denied | Enable the API and add the account as an OAuth test user |
| Upload quota exceeded | Wait for quota reset or request additional quota |
| Upload remains private | This is the default; publish after review |
| Video is skipped | Remove its ID from `pipeline/state.json` only if re-uploading intentionally |

## 12. Maintenance Checklist

Monthly or before a campaign:

- Test all internal pages.
- Check product links and images.
- Review prices and product details.
- Confirm affiliate disclosures.
- Verify the affiliate tag.
- Review YouTube titles and descriptions.
- Back up local credentials securely.
- Keep secrets out of Git.
- Update marketing links after enabling a live site.
