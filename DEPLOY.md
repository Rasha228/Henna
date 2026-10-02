# Deploying Henna By Ayushi

The site is a plain static folder — two HTML pages plus `assets/`. There is no
build step and no server. Anything that can host static files will run it.

Everything in this folder is meant to be published. Working files (screenshots,
the old sage-palette design, the full-resolution logo) live in
`../henna-by-ayushi-dev/` and must **not** be uploaded.

---

## Step 1 — set the real domain (do this first)

Four files contain the placeholder `https://hennabyayushi.com`. They drive the
canonical URL, the social preview image, and the sitemap. Replace it with the
real domain, without a trailing slash:

```bash
cd "C:/Claude Code/henna-by-ayushi" && grep -rl "hennabyayushi.com" . | xargs sed -i "s|https://hennabyayushi.com|https://REAL-DOMAIN-HERE|g"
```

Then confirm nothing was missed:

```bash
grep -rn "hennabyayushi.com" "C:/Claude Code/henna-by-ayushi"
```

If the domain isn't decided yet the site still works — only the social preview
and the sitemap will point at the wrong host until it's fixed.

---

## Step 2 — publish

### Netlify (recommended — easiest)

1. Go to <https://app.netlify.com/drop>
2. Drag the `henna-by-ayushi` folder onto the page.
3. It goes live on a `*.netlify.app` URL within seconds.
4. **Site settings → Domain management → Add a domain** to point the real
   domain at it. HTTPS is issued automatically.

`netlify.toml` and `_headers` in this folder configure caching and security
headers; Netlify picks them up with no extra setup.

### Cloudflare Pages

1. **Workers & Pages → Create → Pages → Upload assets**
2. Upload the folder. `_headers` is read automatically (`netlify.toml` is ignored).
3. Add the custom domain under **Custom domains**.

### GitHub Pages

Works, but ignores `_headers`, so caching is whatever GitHub decides. Fine for a
first launch. Push the folder to a repo and enable Pages on the branch root.

### Traditional cPanel / FTP hosting

Upload the contents of this folder into `public_html`. Caching headers will be
whatever the host's Apache config does — `_headers` and `netlify.toml` are
ignored and can be deleted in that case.

---

## Step 3 — after it's live

- Load the site on a phone, not just a laptop.
- Paste the URL into an Instagram DM or WhatsApp and check the link preview
  shows `assets/og.jpg` and the right title.
- Add the domain to **Google Search Console** and submit `/sitemap.xml`.
- Put the link in the Instagram and TikTok bios.
- Test the cart end to end: add items, reload the page (the cart should survive),
  send an order, confirm the email arrives.

---

## What's in the folder

| File | Purpose |
|---|---|
| `index.html` | Landing page |
| `cones.html` | Shop — cone and bulk pricing, cart |
| `404.html` | Not-found page (served automatically by Netlify/Cloudflare) |
| `assets/` | Photos, reel videos, logo watermark, icons, `tailwind.css` |
| `robots.txt` | Allows crawling, points at the sitemap |
| `sitemap.xml` | Two URLs |
| `_headers` | Cache and security headers (Netlify / Cloudflare Pages) |
| `netlify.toml` | Same, in Netlify's own format |
| `README.md` | How the site is built |
| `DEPLOY.md` | This file |

---

## Notes

**CSS is pre-built.** `assets/tailwind.css` (12 KB) is generated from the two
HTML files. The site no longer uses the Tailwind play CDN, which was never meant
for production. If you add new Tailwind utility classes to the HTML, rebuild it:

```bash
npx tailwindcss@3 -c tailwind.config.js -i input.css -o assets/tailwind.css --minify
```

The config used is recorded at the bottom of `README.md`. Plain CSS edits inside
the `<style>` block in each page need no rebuild.

**Weight.** First load is about 2.3 MB on the landing page and 0.9 MB on the shop
page. Gallery images and the five reel videos are lazy — the videos (8.7 MB
total) only download when someone presses play.

**Fonts** come from Google Fonts. If the client wants zero third-party requests,
they can be self-hosted into `assets/` later.

**Still outstanding before this is truly finished** — see the "Before launch"
section of `README.md`. The testimonials are placeholder text and must be
replaced with real quotes.
