# daily-digest

Static site that hosts a daily curated digest of articles, podcasts, and videos
for [@jesse-spevack](https://github.com/jesse-spevack), generated each morning
by a GitHub Actions workflow.

Live: https://jesse-spevack.github.io/daily-digest

## Layout

```
index.html              Today's digest (also copied into archive/)
archive/                Dated past digests (YYYY-MM-DD.html)
archive/index.html      List of past digests
.nojekyll               Tells GitHub Pages to skip Jekyll processing
```

## How updates happen

`.github/workflows/daily-digest.yml` runs at 13:00 UTC each day (7am MDT;
slides to 6am MST when the US drops DST). The workflow:

1. Installs the Claude Code CLI on a fresh runner.
2. Pipes `.github/digest-prompt.md` into `claude --print` with Read, Write,
   Edit, WebSearch, and Bash tools enabled.
3. The model dedupes against the last 7 archive pages, web-searches for
   fresh content, rewrites `index.html`, copies it to
   `archive/YYYY-MM-DD.html`, and prepends a row to `archive/index.html`.
4. A follow-up step commits the diff as `github-actions[bot]` and pushes
   to `main`. GitHub Pages serves it.

Manual runs: GitHub → Actions → **Daily Digest** → **Run workflow**.

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

The prompt lives at [`.github/digest-prompt.md`](.github/digest-prompt.md).
Edit it on a branch, open a PR, and the next scheduled run picks up the new
behavior.
