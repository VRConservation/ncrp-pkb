# NCRP Proactive Knowledge Base

A searchable, themed website generated from an [Obsidian](https://obsidian.md/)
vault of notes on proactive finance and insurance in the face of climate
risk — wildfire, flooding, sea level rise, and the future of property
insurance. Published automatically to GitHub Pages on every push to `main`.

This repository combines the vault itself (a `notes/` folder of markdown
topic pages) with an [MkDocs Material](https://squidfunk.github.io/mkdocs-material/)
site configuration, and an `AGENTS.md` playbook that [opencode](https://opencode.ai)
follows to file new notes into the right pages automatically.

## Live site

- [ncrp-pkb](https://3point.xyz/ncrp-pkb) — main site
- [Repository](https://github.com/VRConservation/ncrp-pkb)

## What's included

| File / folder | Purpose |
|---|---|
| `notes/` | The vault's published pages — this is the MkDocs `docs_dir` and also just a normal folder of Markdown files, so it works in Obsidian too. |
| `notes/index.md` | Table of contents + "Latest Finds" highlights, shown as the site's home page. |
| `notes/log.md` | Append-only changelog of ingestion operations. |
| `raw/` | Drop new, unprocessed notes here (`New_Notes.md` is the inbox file). |
| `AGENTS.md` | Instructions opencode follows to process notes from `raw/` into topic pages in `notes/`, and to keep the site in sync. |
| `mkdocs.yml` | Site configuration — theme, navigation, plugins. |
| `hooks.py` | Adds a live note-count, e.g. `Insurance Fire (20)`, next to each topic in the site navigation. |
| `requirements.txt` | Pinned Python packages needed to build the site. |
| `.github/workflows/deploy.yml` | GitHub Actions workflow that builds and deploys the site to GitHub Pages on every push to `main`. |
| `.obsidian/` | Minimal Obsidian vault config, so this folder opens as a vault immediately. |

## Topics

The vault organizes notes around property insurance and resilience under
climate risk. See `notes/index.md` for the current table of contents:

- Insurance (hub)
- Insurance Fire — FAIR Plan crisis, mitigation discounts, home hardening
- Insurance Flood — NFIP, community-based flood insurance, parametric products
- Insurance Climate — insurability, climate disclosures
- Insurance Community — community-based catastrophe insurance (CBCI)
- Insurance Parametric — index-based insurance
- Insurance Solutions — proactive products, building back stronger
- Insurance Nature — nature-based solutions and risk reduction
- Insurance Funding — municipal bonds, resilience investment
- Insurance Data — catastrophe modeling, WUI Data Commons
- Sea Level Rise
- Flooding
- Fire

## Adding notes

Drop a note (a URL, a clipped article, or plain text) into `raw/New_Notes.md`,
then ask opencode to ingest it. It will read `AGENTS.md`, find or create the
right topic page, add the entry with backlinks and keywords, update
`notes/index.md` and `notes/log.md`, and sync the site.

## Local development

```bash
python3 -m venv .venv && source .venv/bin/activate
pip install -r requirements.txt
mkdocs serve
```

Open `http://127.0.0.1:8000` to preview. To build a static copy:

```bash
mkdocs build
```

## Deploying

Push to `main`; the included `.github/workflows/deploy.yml` builds the site
with MkDocs and deploys it to GitHub Pages. Ensure GitHub Pages is set to
**Settings → Pages → Build and deployment → Source → GitHub Actions** in the
repo settings.