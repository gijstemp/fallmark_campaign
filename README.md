# The Fracturing: Player Guide

A player-facing companion site for the campaign, built as a plain Jekyll site (no external theme dependency; the parchment/sourcebook look in `assets/css/style.css` is hand-built) so it deploys on GitHub Pages with zero local build tooling required.

## ⚠️ Keep this repo separate, always

This folder is deliberately its own git repository, independent from the main campaign vault. **Never** add this as a subfolder of a repo that also contains the DM vault (world lore, BBEG files, one-shots, session prep). Everything in here is meant to be public or player-shareable; everything in the vault is not. If you ever restructure this, keep that boundary intact.

## First-time setup: getting this on GitHub Pages

You'll need a GitHub account and the `git` CLI (already initialized in this folder, see below).

**1. Create a new, empty repository on GitHub.**
Do *not* initialize it with a README, license, or .gitignore; this folder already has those. Two naming options:

- Name it `<your-github-username>.github.io` so the site publishes at the root, e.g. `https://yourname.github.io/`. With this option, leave `baseurl: ""` in `_config.yml` exactly as it is now.
- Name it anything else, e.g. `the-fracturing-guide`, so the site publishes at `https://yourname.github.io/the-fracturing-guide/`. With this option, **edit `_config.yml`** and set:
  ```yaml
  baseurl: "/the-fracturing-guide"
  url: "https://yourname.github.io"
  ```
  (match the repo name exactly, including case). Every internal link on the site uses Jekyll's `relative_url` filter, so this one edit is all that's needed; nothing else has hardcoded paths.

**2. Point this local repo at it and push:**
```bash
cd player-site
git remote add origin https://github.com/<your-username>/<repo-name>.git
git branch -M main
git push -u origin main
```

**3. Enable Pages.** In the new repo on GitHub: **Settings → Pages → Build and deployment → Source: Deploy from a branch → Branch: `main`, folder: `/ (root)`**. Save. GitHub builds the Jekyll site automatically server-side, with no Actions workflow, no Ruby install, and no local build step required. First deploy usually takes a minute or two; check the "Actions" tab on the repo for build status if the site doesn't appear right away.

**4. Share the URL** (`https://yourname.github.io/` or `https://yourname.github.io/repo-name/`) with your players.

## Previewing locally (optional)

Only needed if you want to check changes before pushing. Requires Ruby + Bundler:
```bash
gem install bundler jekyll
bundle init
bundle add jekyll
bundle exec jekyll serve
```
Then visit `http://localhost:4000`. Entirely optional; GitHub will build and publish correctly without ever doing this.

## Updating after each session

1. Open `sessions/index.md` and add a new `.session-entry` block above the HTML comment at the bottom, following the format shown in the comment. Keep recaps player-facing: no DM-only information (motives, secrets, or anything the party hasn't actually learned in-fiction).
2. Edit any other page if the campaign has revealed something worth adding, such as a new region or a faction's public reputation shifting. Same rule: only what the *players* would reasonably know.
3. Commit and push:
   ```bash
   git add -A
   git commit -m "Add Session N recap"
   git push
   ```
   GitHub rebuilds the live site automatically within a minute or two of the push.

## Content boundary: what belongs on this site

Safe to add: anything the party has directly experienced or that's genuinely public in-world knowledge (regions, cultures, factions' public faces, session recaps, house rules, character-creation notes).

Never add: BBEG identities or motives, one-shot/adventure contents not yet run, hidden faction agendas, NPC secrets, or anything from the DM vault's `npcs/bbeg/`, `npcs/villains/`, `one-shots/`, or `lore/` files beyond what's already been revealed at the table. When in doubt, check with the DM side of the project before publishing something here.

## Structure

```
player-site/
├── _config.yml            site settings, nav menu
├── _layouts/default.html  the one page template
├── _includes/              head, nav, footer partials
├── assets/css/style.css   the parchment theme (all hand-written, no external theme)
├── assets/images/          world-map.jpg (optimized web copy)
├── index.md               -> /
├── world/index.md         -> /world/
├── fracturing/index.md    -> /fracturing/
├── peoples/index.md       -> /peoples/
├── factions/index.md      -> /factions/
├── gods/index.md          -> /gods/
├── band-life/index.md     -> /band-life/
├── table/index.md         -> /table/
├── characters/index.md    -> /characters/
└── sessions/index.md      -> /sessions/  (grows after every session)
```

To add a brand-new top-level page, create `new-folder/index.md` with front matter (`layout: default`, a unique `nav_key`, `permalink: /new-folder/`, `title`), then add a matching entry to the `nav` list in `_config.yml`.
