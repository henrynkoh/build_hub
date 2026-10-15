# Build Hub: Construction Project Hub

Build Hub is a static construction-planning website that organizes a residential build into 15 workflow steps. It combines material and labor estimates, supplier information, Amazon affiliate product picks, and a 60-video Shorts Studio. An optional macOS-oriented Python pipeline renders and uploads YouTube Shorts.

Repository: https://github.com/henrynkoh/build_hub

[![Open Build Hub Landing Page](https://img.shields.io/badge/OPEN-LANDING_PAGE-238636?style=for-the-badge&logo=githubpages&logoColor=white)](https://henrynkoh.github.io/build_hub/)

## Highlights

- 15 construction workflow pages, from site preparation through final punch list
- 69 material lines with quantity, price, labor-hour, and supplier fields
- 180 Amazon product picks organized by construction step
- Affiliate campaign page and CSV export
- 60-video browser-based Shorts Studio
- Optional Python rendering and YouTube upload pipeline
- Plain HTML, CSS, and JavaScript with no framework or build step

## Quick Start

```bash
git clone https://github.com/henrynkoh/build_hub.git
cd build_hub
python3 -m http.server 8000
```

Open http://localhost:8000.

Read next:

- [5-minute Quick Start](docs/QUICKSTART.md)
- [Step-by-step Tutorial](docs/TUTORIAL.md)
- [Complete Manual](docs/MANUAL.md)
- [Marketing Kit](marketing/README.md)

## Project Structure

```text
build_hub/
├── index.html
├── step-01.html ... step-15.html
├── affiliate.html
├── shorts.html
├── data.js
├── app.js
├── styles.css
├── shorts-data.js
├── pipeline/
│   ├── run_pipeline.py
│   └── shorts.json
├── docs/
│   ├── QUICKSTART.md
│   ├── TUTORIAL.md
│   └── MANUAL.md
└── marketing/
    ├── README.md
    ├── facebook.md
    ├── instagram.md
    ├── threads.md
    ├── blogger.md
    ├── naver-blog.md
    ├── tistory.md
    ├── wordpress.md
    ├── newsletter.md
    └── email.md
```

## Main Pages

| Page | Purpose |
|---|---|
| `index.html` | Main hub, workflow, materials, tools, costs, and labor |
| `step-01.html` to `step-15.html` | Detailed page for each construction step |
| `affiliate.html` | Amazon affiliate campaign and CSV export |
| `shorts.html` | Preview and browser rendering interface for 60 Shorts |

The website depends on `data.js`, `app.js`, `styles.css`, and `shorts-data.js`.

## Customize the Amazon Associates Tag

The current tag is `buildsmartpic-20`. Replace it in:

- `data.js`
- `shorts-data.js`
- `pipeline/shorts.json`

Check for remaining occurrences:

```bash
grep -rn "buildsmartpic-20" .
```

You can also override the tag for a pipeline run with `--tag`.

## YouTube Shorts Pipeline

The pipeline uses the macOS `say` command for narration.

### Requirements

- macOS
- Python 3
- ffmpeg
- Pillow
- requests
- google-api-python-client
- google-auth-oauthlib

```bash
brew install ffmpeg
python3 -m venv pipeline/.venv
source pipeline/.venv/bin/activate
pip install Pillow requests google-api-python-client google-auth-oauthlib
```

Render one test Short:

```bash
python3 pipeline/run_pipeline.py \
  --tag buildsmartpic-20 \
  --render \
  --only s01-m-1
```

For YouTube uploads, place the OAuth Desktop App file at `pipeline/client_secret.json`. The script stores authorization at `pipeline/token.json`. Never commit either file.

```bash
python3 pipeline/run_pipeline.py \
  --tag buildsmartpic-20 \
  --upload \
  --per-day 6 \
  --privacy private
```

Uploaded videos default to private. Review each video in YouTube Studio before publishing.

## Disclosures and Safety

- Clearly disclose Amazon affiliate links. Suggested wording: "As an Amazon Associate I earn from qualifying purchases."
- Product prices, ratings, availability, and rankings can change. Verify before publishing or purchasing.
- Construction quantities and labor figures are planning estimates, not bids or engineering documents.
- Follow local building codes, permit requirements, manufacturer instructions, and job-site safety practices.
- Consult licensed professionals when required.

## Credential Safety

Never commit credentials or authorization tokens. Recommended `.gitignore` entries:

```gitignore
pipeline/client_secret.json
pipeline/token.json
pipeline/state.json
pipeline/out/
*.pickle
```

If a credential is exposed, revoke it immediately, generate a replacement, and remove the exposed value from Git history.

## License

No license has been specified. Add a `LICENSE` file before allowing reuse or redistribution.
