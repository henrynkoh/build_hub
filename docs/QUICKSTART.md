# Build Hub Quick Start

Get the website running in about five minutes.

## 1. Download the Project

```bash
git clone https://github.com/henrynkoh/build_hub.git
cd build_hub
```

You can also use GitHub's **Code > Download ZIP** option and extract the archive.

## 2. Start a Local Web Server

```bash
python3 -m http.server 8000
```

Open:

- Home: http://localhost:8000
- Affiliate campaign: http://localhost:8000/affiliate.html
- Shorts Studio: http://localhost:8000/shorts.html

Stop the server with `Control-C`.

## 3. Set Your Affiliate Tag

Replace `buildsmartpic-20` in:

- `data.js`
- `shorts-data.js`
- `pipeline/shorts.json`

Verify the replacement:

```bash
grep -rn "buildsmartpic-20" .
```

## 4. Test the Website

Confirm that:

- The home page displays 15 workflow steps.
- Material and tool tables load.
- A step page opens.
- The Affiliate page shows product cards.
- Shorts Studio lists 60 videos.

## 5. Optional: Render One Short on macOS

```bash
brew install ffmpeg
python3 -m venv pipeline/.venv
source pipeline/.venv/bin/activate
pip install Pillow requests google-api-python-client google-auth-oauthlib
python3 pipeline/run_pipeline.py \
  --tag buildsmartpic-20 \
  --render \
  --only s01-m-1
```

Rendered files are written under `pipeline/out/`.

## 6. Optional: Upload to YouTube

1. Enable YouTube Data API v3 in Google Cloud.
2. Create an OAuth Desktop App.
3. Save the downloaded file as `pipeline/client_secret.json`.
4. Keep it out of Git.
5. Run:

```bash
python3 pipeline/run_pipeline.py \
  --tag buildsmartpic-20 \
  --upload \
  --per-day 1 \
  --privacy private \
  --only s01-m-1
```

Review the private upload in YouTube Studio.

## Next

- [Tutorial](TUTORIAL.md)
- [Full Manual](MANUAL.md)
- [Marketing Kit](../marketing/README.md)
