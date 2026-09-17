# Website Sandbox

A playground for small, stupid, self-contained websites. Build one, host it, tear it
down, build another. Nothing in here is serious and nothing in here is load-bearing.

Deliberately **not** part of [World_Cup_Tracker](https://github.com/TallGibbs/World_Cup_Tracker),
which has a weekly scheduled routine, a validator that gates every commit, and a
production Cloudflare Pages deploy wired to `main`. Toys don't belong in there.

## Ground rules

1. **One folder per site.** Each has its own `index.html` and its own `assets/`.
   No shared CSS, no shared JS, no build step, no framework, no package.json.
2. **Self-contained.** A site should work by double-clicking its `index.html`.
   That means no CDN links and no external fonts.
3. **Deleting a folder deletes the site.** That is the entire point.
4. **Never commit raw email.** `.eml` and `.msg` are gitignored because they carry
   everyone's addresses, phone numbers, and mailing addresses in the headers. Keep
   originals in `_source/` (also gitignored) and copy only what you need into
   a site's `assets/`.

## Sites

| Path | What it is |
| --- | --- |
| [`strangers-2026/`](strangers-2026/) | **Delores Threat Board.** A mock situation board for the Never Get In A Van With Strangers relay team. Started life memorialising Kelaine's Aug 2026 email, when Ragnar Trail Wisconsin might or might not have been cancelled; updated 17 Sept 2026 when the race was confirmed and Chicago thunderstorms started holding half the inbound roster. Live countdown, arrivals manifest, rumour chain-of-custody, the nightmare roster, and a raccoon. |

## Local preview

Most of these work by opening `index.html` directly. If you want clean URLs and
correctly-served assets, serve the folder:

```bash
python -m http.server 8777
```

Then visit `http://localhost:8777/`. Stop it with Ctrl+C.

## Deploying (Cloudflare Pages)

Same pattern as the World Cup tracker, second project. There is no build step —
Cloudflare publishes the repo root as-is.

1. Push this repo to GitHub.
2. Cloudflare dashboard → **Workers & Pages** → **Create** → **Pages** →
   **Connect to Git** → pick this repo.
3. Build settings: **Framework preset:** None. **Build command:** leave empty.
   **Build output directory:** `/`.
4. Custom domain → `sandbox.youmissedit.org` (Cloudflare adds the CNAME itself,
   since `youmissedit.org` is already on Cloudflare DNS).

Each site is then served natively at its folder path — `/strangers-2026/` — with
no `_redirects` rule needed. **Don't add one.** Pages canonicalises `.html` back to
the clean URL, so a `/foo -> /foo.html` rule creates an infinite redirect loop.

The free tier allows 500 builds/month and has no bandwidth cap, so a sandbox that
gets pushed to constantly is fine.

### A note on visibility

Pages projects are public by default — anyone with the URL can read them, and
`sandbox.youmissedit.org/strangers-2026/` is a guessable URL. Every page here
carries `<meta name="robots" content="noindex, nofollow">`, which keeps it out of
search results but is not access control. Assume anything you deploy is readable
by anyone who is handed the link, and keep personal contact details out of the
markup accordingly. If you want real protection, put Cloudflare Access in front of
the project.
