# Deploy — Ōtsuka Summer guide

This folder holds a single static site, `index.html`. No build step, no framework — deploying it means putting one file on a host and getting a public URL back.

---

## For Claude Code (read this first)

- **Goal:** publish `index.html` in this folder to a free static host and return the live URL.
- **Files:** `index.html` (the whole site) + this `README.md`.
- **Your job:** run the commands under the host the user picked, then paste back the live link.
- **Human-only steps:** anything that logs in or creates an account (`netlify login`, `gh auth login`, `wrangler login`, etc.). Pause and let the user complete those in the browser — don't attempt to enter credentials.
- **Recommended default:** Netlify (fastest to a live URL). Use GitHub Pages if the user wants a permanent, memorable address.
- **Redeploys:** after any edit to `index.html`, re-run the same host's deploy/push command (see "Updating" at the bottom).

---

## Prerequisites

- **Node.js** installed (`node -v` should print a version). If missing, install the LTS build from nodejs.org.
- This folder open in a terminal, with `index.html` present (`ls` / `dir` to confirm).

---

## Option A — Netlify (fastest, instant URL)

```bash
npm install -g netlify-cli
netlify login                       # opens the browser — user authorises (human step)
netlify deploy --dir . --prod       # from THIS folder; prompts to create a new site the first time
```

- On the first run it asks to create/link a site — choose **create a new site**, accept the defaults.
- When it finishes it prints a **Website URL** (e.g. `https://otsuka-summer.netlify.app`). That's the public link.

No-terminal alternative: go to `app.netlify.com/drop` and drag `index.html` onto the page.

---

## Option B — GitHub Pages (permanent, free address)

Gives a stable URL like `https://<your-username>.github.io/otsuka-summer/`.

```bash
gh auth login                       # human step — pick GitHub.com, HTTPS, log in via browser
git init
git add index.html README.md
git commit -m "Ōtsuka Summer Tokyo guide"
gh repo create otsuka-summer --public --source=. --push
```

Then turn Pages on (either way works):

```bash
# via CLI:
gh api -X POST repos/{owner}/otsuka-summer/pages \
  -f "source[branch]=main" -f "source[path]=/"
# (replace {owner} with your GitHub username)
```

…or in the browser: **repo → Settings → Pages → Branch: `main` / folder: `/ (root)` → Save**.

The site goes live at `https://<username>.github.io/otsuka-summer/` within a minute or two. GitHub serves `index.html` automatically.

---

## Option C — Cloudflare Pages

```bash
npm install -g wrangler
wrangler login                      # human step — browser auth
wrangler pages deploy . --project-name otsuka-summer
```

It prints a `*.pages.dev` URL when done.

---

## Updating the site later

Edit `index.html` (all the content lives in the `PLACES` list near the top of its script), then redeploy:

| Host           | Command to push an update                          |
|----------------|----------------------------------------------------|
| Netlify        | `netlify deploy --dir . --prod`                    |
| GitHub Pages   | `git add -A && git commit -m "update" && git push` |
| Cloudflare     | `wrangler pages deploy . --project-name otsuka-summer` |

Or just tell me what changed and I'll regenerate `index.html` for you to redeploy.

---

## Custom domain (optional)

All three hosts support adding your own domain in their dashboard under the site's **Domain / DNS** settings — point a `CNAME` at the host's target, and they issue HTTPS automatically. Ask Claude Code to walk you through it once the site is live.

---

## Troubleshooting

- **Command not found** after `npm install -g` — restart the terminal, or re-run so the global path is picked up.
- **Login stuck** — these open a browser; complete the login there, then the terminal continues.
- **Wrong flag / CLI changed** — run the tool with `--help` (e.g. `netlify deploy --help`) to see the current form; the steps above are otherwise standard.
- **Blank page when hosted** — confirm the file is named exactly `index.html` at the site root.
