# Hidden Pace — website

Static site, no build step, no JavaScript, no cookies. Style follows the game's "Alpine '72" tokens (`hiddenpace/Design/Game/GameTheme.swift`).

| Page | File | App Store Connect field |
|---|---|---|
| Home | `index.html` | Marketing URL |
| Support (contact + FAQ) | `support.html` | Support URL (required) |
| Privacy Policy | `privacy.html` | Privacy Policy URL (required) |
| Terms of Use | `terms.html` | — (licence = Apple Standard EULA, linked from the page) |
| Not found | `404.html` | — |

Links between pages are relative (`privacy.html`), so the site works on any host and in a sub-path. Canonical URLs, `sitemap.xml` and `robots.txt` assume **https://hiddenpace.github.io** — change them if the site lives elsewhere.

`_headers` (security headers, image caching) is used by Cloudflare Pages / Netlify; `.nojekyll` keeps GitHub Pages from running Jekyll. Images: `icon.png` from `Tools/app-icon/preview.png`, `images/*.jpg` from `appstore/en/screenshots/` (600 px, `sips`), `images/ridges.svg` — the game's ridge layers.

## Publish

**Cloudflare Pages** (like afterlap.app): new project → upload the `site/` folder or connect the repo with build output directory `site`, no build command. Add the custom domain `hiddenpace.app`. Clean URLs (`/privacy`) work out of the box.

**GitHub Pages**: Pages needs a public repo on the free plan. Either publish this repo with "Deploy from a branch → main → /site" (requires moving the folder to `/docs` or using an Actions workflow), or copy `site/` into a small public repo (e.g. `jacifkusto/hiddenpace-site`). URLs: `https://jacifkusto.github.io/hiddenpace-site/privacy`.

## After publishing

1. Open `/privacy` and `/support` in a browser — App Review checks both.
2. Put the URLs into `Tools/asc/listing.py` → `APP`:
   ```python
   "marketing_url": "https://hiddenpace.github.io",
   "support_url": "https://hiddenpace.github.io/support",
   "privacy_url": "https://hiddenpace.github.io/privacy",
   ```
3. `Tools/asc/deploy_metadata.py` (dry run) → `Tools/asc/deploy_metadata.py --apply`. The URL fields go to all 15 locales and the "Privacy Policy / Support" lines return to the descriptions.

## Keeping it true

The Privacy Policy describes what the app does today: no server, no SDKs, local saves with weekly backups, optional iCloud (private CloudKit + KVS), StoreKit, Game Center, local reminder, share sheet. If any of that changes (analytics, crash reporting, a backend), update `privacy.html`, its effective date and the App Store privacy label before shipping.
