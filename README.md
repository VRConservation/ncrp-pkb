# Personal Notes Website Template

Turn an [Obsidian](https://obsidian.md/) vault into a searchable, themed
notes website — published automatically to GitHub Pages every time you
push. This repository is a **GitHub template**: click "Use this template"
to get your own copy pre-populated with a working Obsidian vault
structure, a matching [MkDocs Material](https://squidfunk.github.io/mkdocs-material/)
site configuration, and a `CLAUDE.md` playbook that [Claude Code](https://claude.com/claude-code)
follows to file new notes into the right pages automatically.

It ships with 12 placeholder topic pages (lorem ipsum content) so you can
see the exact format expected, then replace them with your own notes.

## Who this is for

Anyone who keeps notes in Obsidian and wants a low-maintenance public (or
private) website generated from those notes, without hand-building a
static site or writing publishing scripts. It's especially suited to
people who use Claude Code (or a similar coding agent) as their note-taking
assistant — the included `CLAUDE.md` defines the exact ingestion workflow
an agent should follow, so "add a note" becomes a one-line request instead
of manual file editing.

## What's included

| File / folder | Purpose |
|---|---|
| `notes/` | Your vault's published pages — this is the MkDocs `docs_dir` and also just a normal folder of Markdown files, so it works in Obsidian too. |
| `notes/index.md` | Table of contents + "Latest Finds" highlights, shown as the site's home page. |
| `notes/log.md` | Append-only changelog of ingestion operations. |
| `raw/` | Drop new, unprocessed notes here (`New_Notes.md` is the inbox file). |
| `CLAUDE.md` | Instructions Claude Code follows to process notes from `raw/` into topic pages in `notes/`, and to keep the site in sync. |
| `mkdocs.yml` | Site configuration — theme, navigation, plugins. |
| `hooks.py` | Adds a live note-count, e.g. `Python (3)`, next to each topic in the site navigation. |
| `requirements.txt` | Pinned Python packages needed to build the site. |
| `.github/workflows/deploy.yml` | GitHub Actions workflow that builds and deploys the site to GitHub Pages on every push to `main`. |
| `.obsidian/` | Minimal Obsidian vault config, so this folder opens as a vault immediately. |

## Prerequisites

- [Obsidian](https://obsidian.md/) (optional, but this is designed as a vault you can also edit visually)
- Python 3.10+ and `pip` (or [`uv`](https://github.com/astral-sh/uv))
- `git`
- A GitHub account
- [GitHub CLI](https://cli.github.com/) (`gh`) — optional, only needed if you want to create the repo from the command line instead of the website

## Setup

### 1. Create your copy

Click **Use this template → Create a new repository** at the top of this
page, or from the command line:

```bash
gh repo create YOUR_USERNAME/YOUR_REPO --template spatialthoughts/personal-notes-website-template --public --clone
cd YOUR_REPO
```

### 2. Point the config at your repo

Edit `mkdocs.yml` and replace every occurrence of `YOUR_USERNAME` and
`YOUR_REPO` with your actual GitHub username and repository name, and set
`site_name` to whatever you want the site to be called:

```yaml
site_name: Your Notes
site_url: https://YOUR_USERNAME.github.io/YOUR_REPO/
repo_url: https://github.com/YOUR_USERNAME/YOUR_REPO
repo_name: YOUR_USERNAME/YOUR_REPO
```

(`edit_uri: edit/main/notes/` assumes your default branch is `main` — update it if you use a different default branch.)

### 3. Install dependencies and preview locally

```bash
python3 -m venv .venv && source .venv/bin/activate   # or: uv venv && source .venv/bin/activate
pip install -r requirements.txt                        # or: uv pip install -r requirements.txt
mkdocs serve
```

Open `http://127.0.0.1:8000` — you should see the placeholder site with
all 12 topic pages. Edit any file in `notes/` and the preview reloads
live.

### 4. Enable GitHub Pages

In your new repo on GitHub: **Settings → Pages → Build and deployment →
Source → GitHub Actions**. No further configuration needed — the included
`.github/workflows/deploy.yml` builds the site with MkDocs and deploys it
whenever you push to `main`.

### 5. Push

```bash
git add -A
git commit -m "Customize mkdocs.yml for my site"
git push
```

Check the **Actions** tab for the deploy run; once it finishes, your site
is live at the `site_url` you set in step 2.

## Already Have an Obsidian Vault?

If you already keep notes in Obsidian with a similar structure (a `notes/` folder of topic
pages, ideally with a `CLAUDE.md` describing your own topics and workflow), just ask Claude
Code to bring this template into your vault — you don't need to manually copy any files or
start from the placeholder content.

### Ask Claude Code

Open Claude Code in your vault directory and ask something like:

> Use the template at https://github.com/spatialthoughts/personal-notes-website-template to
> publish this vault as a website. Fetch its mkdocs.yml, hooks.py, requirements.txt,
> .github/workflows/deploy.yml, and .gitignore, and add them here (merging .gitignore with
> mine if I already have one). Then:
> 1. Confirm the GitHub repo name for the notes website.
> 3. Update mkdocs.yml's `nav:` list to match my actual topic pages instead of the
>    template's placeholder ones, and set site_name/site_url/repo_url/repo_name for my repo
>    at github.com/USERNAME/YOUR_REPO.
> 4. If notes/index.md doesn't already have a "Latest Finds" section, add one above my
>    existing content. Move the existing content under a "Topics" section.
> 5. Merge the "Update the Website" section from the template's CLAUDE.md into my own
>    CLAUDE.md so future note ingestions keep the site in sync.
> Then run a local mkdocs build to confirm it works.

Claude Code fetches the relevant files from the template repo itself (e.g. via `git clone`
into a temp directory, or reading the raw file contents), adapts them to your actual vault
structure, and leaves your existing notes untouched.

### Then follow the same Setup steps

Once the config points at your vault and your repo, follow steps 3–5 under Setup above:
install dependencies, preview locally with `mkdocs serve`, enable GitHub Pages
(Settings → Pages → Source → GitHub Actions), and push.

## Replacing the placeholder content

Every file in `notes/` currently contains lorem-ipsum sample entries so
you can see the exact expected format. Open `notes/Python.md` (or any
other topic page) as a reference, then either:

- **Edit by hand** in Obsidian or any text/Markdown editor, following the
  format documented in `CLAUDE.md` under "Topic Page Format", or
- **Use Claude Code**: drop a note (a URL, a clipped article, or plain
  text) into `raw/New_Notes.md`, then ask Claude Code to ingest it. It
  will read `CLAUDE.md`, find or create the right topic page, add the
  entry with backlinks and keywords, update `notes/index.md`, and log the
  change in `notes/log.md` — following the exact workflow this template
  ships with. See `CLAUDE.md` for the full playbook and the list of
  starter topics (Technology: Machine Learning, Embeddings, Deep
  Learning, SQL, Python, Data, Agentic Coding; Thematic: Climate Change,
  Urban Planning, Agriculture, Remote Sensing, Cartography) — feel free
  to rename, remove, or add topics to match your own interests, keeping
  the `Title_Case_With_Underscores.md` filename convention.

Whenever you add or remove a topic page, also update the `nav:` list in
`mkdocs.yml` to match.

## Customizing the site

- **Theme colors**: edit the `theme.palette` block in `mkdocs.yml` (any
  [Material for MkDocs color](https://squidfunk.github.io/mkdocs-material/setup/changing-the-colors/)).
- **Navigation style**: `theme.features` in `mkdocs.yml` controls tabs,
  instant loading, search behavior, etc. — see the
  [Material for MkDocs docs](https://squidfunk.github.io/mkdocs-material/setup/).
- **Nav note counts**: `hooks.py` automatically appends `(N)` to each
  topic in the nav based on how many `- [` bulleted entries are in its
  page — no changes needed unless you change the note-entry format.
- **Cross-links between pages**: use Obsidian `[[Wikilink]]` syntax
  anywhere in a topic page (e.g. `[[Python]]`); the `roamlinks` MkDocs
  plugin resolves these into working links at build time, and Obsidian
  renders them natively when you browse the vault.

## Reference implementation

See the actively-maintained vault published at
[spatialthoughts.github.io/notes](https://spatialthoughts.github.io/notes/)  for an example of what a populated site looks like in
practice.

## License

Feel free to use, modify, and redistribute this template for your own
notes site.
