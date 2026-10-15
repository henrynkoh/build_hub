# Build Hub Tutorial

This walkthrough shows how to run the site, understand its files, customize the affiliate tag, render a test Short, and prepare for deployment.

## Part 1: Run the Site

```bash
git clone https://github.com/henrynkoh/build_hub.git
cd build_hub
python3 -m http.server 8000
```

Open http://localhost:8000.

The top navigation links to Home, Workflow, Materials, Tools, Affiliate, and Shorts Studio.

## Part 2: Explore the Construction Workflow

1. Review the 15-step workflow on the home page.
2. Open `step-01.html` and confirm the materials, tools, labor, and affiliate picks load.
3. Use the previous and next links to move through the workflow.
4. Return to the home page and test the material search and supplier filters.

The website content comes from:

- `data.js`: construction data, product picks, and affiliate tag
- `app.js`: rendering and interactions
- `styles.css`: visual design
- `shorts-data.js`: the 60 Shorts definitions used by the browser studio

## Part 3: Customize the Affiliate Tag

Open these three files:

- `data.js`
- `shorts-data.js`
- `pipeline/shorts.json`

Replace `buildsmartpic-20` with your Amazon Associates tracking ID.

```bash
grep -rn "buildsmartpic-20" .
```

Reload the site and inspect a product link. It should contain your tag after `?tag=`.

Keep the required affiliate disclosure visible on pages and in video descriptions.

## Part 4: Customize Content

### Construction data

Edit `data.js` to change:

- Workflow names and descriptions
- Material quantities and prices
- Labor hours
- Suppliers
- Amazon product picks

### Shorts data

Keep `shorts-data.js` and `pipeline/shorts.json` synchronized so the browser Studio and Python pipeline describe the same videos.

### Design

Edit `styles.css` for colors, spacing, cards, tables, and mobile layout.

## Part 5: Render a Test Short

The included Python pipeline is macOS-oriented because it uses the built-in `say` command.

```bash
brew install ffmpeg
python3 -m venv pipeline/.venv
source pipeline/.venv/bin/activate
pip install Pillow requests google-api-python-client google-auth-oauthlib
```

Render only the first Short:

```bash
python3 pipeline/run_pipeline.py \
  --tag buildsmartpic-20 \
  --render \
  --only s01-m-1
```

Review the MP4 under `pipeline/out/`. Check narration, text placement, image quality, affiliate disclosure, and safe margins before rendering the full set.

## Part 6: Configure YouTube Uploads

1. Create a Google Cloud project.
2. Enable YouTube Data API v3.
3. Configure the OAuth consent screen.
4. Create an OAuth Client ID for a Desktop app.
5. Save the file as `pipeline/client_secret.json`.
6. Add your YouTube account as a test user if the OAuth app is in testing mode.

Upload one private test video:

```bash
python3 pipeline/run_pipeline.py \
  --tag buildsmartpic-20 \
  --upload \
  --per-day 1 \
  --privacy private \
  --only s01-m-1
```

The first run opens a browser for authorization and creates `pipeline/token.json`. The upload state is saved in `pipeline/state.json`.

## Part 7: Publish the Static Website

The repository can be deployed to any static host. For GitHub Pages, select the `main` branch and repository root in **Settings > Pages**. Use the URL GitHub provides after deployment.

Before promoting the site:

- Test every page and link.
- Confirm your affiliate tag.
- Confirm disclosures.
- Verify product information.
- Remove or ignore credentials.
- Review mobile layout.

## Part 8: Use the Marketing Kit

Open [marketing/README.md](../marketing/README.md) for Facebook, Instagram, Threads, Blogger, Naver Blog, Tistory, WordPress, newsletter, and email templates.
