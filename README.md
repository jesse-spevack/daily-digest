# daily-digest

Static site that hosts a daily curated digest of articles, podcasts, and videos
for [@jesse-spevack](https://github.com/jesse-spevack), generated each morning by a
Cowork-mode scheduled task.

Live: https://jesse-spevack.github.io/daily-digest

## Layout

```
index.html              Today's digest (also copied into archive/)
archive/                Dated past digests (YYYY-MM-DD.html)
archive/index.html      List of past digests
.nojekyll               Tells GitHub Pages to skip Jekyll processing
```

## How updates happen

A scheduled Cowork task runs at 07:00 America/Denver each day:

1. Reads recent items from `notes.verynormal.dev` via MCP to understand current taste
   and avoid duplicates.
2. Searches the web for fresh content (last 48–72h preferred) across the heavy lane
   (AI, tech business, engineering, microsaas, Rails) plus one wildcard.
3. Writes the new `index.html` and `archive/YYYY-MM-DD.html`, regenerates
   `archive/index.html`.
4. Commits and pushes to `main`. GitHub Pages serves it.

## Manual rebuild

If something looks wrong:

```bash
cd ~/code/daily-digest
git pull
# fix files
git add -A
git commit -m "manual edit"
git push
```

## Tweaking the curation

The digest content rules live in the scheduled task prompt managed by Cowork
(task id `daily-interesting-digest`). Edit it from the Scheduled tab in the app,
or ask Claude to update it.
