# switchboard-site

Source for the Switchboard marketing/waitlist page, served at
[switchboard.kids](https://switchboard.kids) by the Cloudflare Worker
`switchboard`.

## What's here
- `index.html` — the whole site. No build step, no framework. Static HTML/CSS
  with a small inline `<script>` for the waitlist form (posts to the parent
  portal's `/api/waitlist`, falls back to Formspree if the portal is
  unreachable; includes a honeypot field against spam).
- `wrangler.toml` — Workers **Static Assets** config (the current-recommended
  approach; Workers *Sites* is deprecated). `directory = "./"` means the whole
  repo root is served as-is.

## Deploy pipeline
Push to `main` on GitHub → Cloudflare's **Workers Builds** (native Git
integration, connected in the dashboard under Workers & Pages → `switchboard`
→ Settings → Build) builds and deploys automatically. Non-`main` branches get
a preview URL before anything goes live. No local `wrangler deploy`, no
drag-and-drop upload — that manual step is what let the live site go stale for
weeks before this pipeline existed (see the project's `server-audit-findings.md`,
2026-08-07 entry, for the full story).

To ship a change: edit `index.html`, commit, push to `main`. That's it.

## Why this repo lives here and not in the main project folder
This project's main folder (`Universal Design and Manufacturing/`, the
OneDrive-synced Switchboard project root) is **not** a git repo and shouldn't
become one casually — it has secrets sitting in it (SSH key, registrar
recovery codes — flagged as a separate open item, not fixed by this repo) and
OneDrive's continuous re-sync doesn't mix well with a live `.git` folder. This
repo is deliberately scoped narrow (one HTML file + one config file) and lives
locally at `C:\Users\dadad\dev\switchboard-site\` — outside OneDrive/Documents/
Desktop, so Windows doesn't sync it. `C:\Users\dadad\dev\` is this project's
convention for anything that needs a real non-synced local checkout; this repo
is the first thing to use it.

## Local checkout / GitHub
- Local path: `C:\Users\dadad\dev\switchboard-site\`
- Remote: private GitHub repo (see `INFRA.md` in the main project folder for
  the exact URL and account)

## Custom domain
`switchboard.kids` and `www` are attached to the `switchboard` Worker as
custom domains (configured in the Cloudflare dashboard, not in this repo).
This repo's push-to-deploy pipeline updates the content served at that domain;
it doesn't touch DNS or the domain attachment itself.
