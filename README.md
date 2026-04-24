# iantrimble.com — Lectern AI landing page

Static single-page site that showcases the Lectern AI Mac app, plus the
legal pages the Mac App Store requires (privacy, terms, support).

Lives at `/Volumes/nas/projects/app-website/` on the Mac, which is the
same directory as `/volume1/nas/projects/app-website/` on the Ugreen NAS.

## Files

```
index.html              Main landing page
privacy.html            Privacy policy (Mac App Store requirement)
terms.html              Terms of Use (Mac App Store requirement)
support.html            Support / contact
favicon.svg             Brand-coloured "L" icon
manifest.json           PWA / homescreen icon manifest
sitemap.xml             Single-page sitemap
robots.txt              Allow all + sitemap pointer
assets/legal.css        Shared styling for sub-pages
press-kit/              (to fill) logos, screenshots, brand guide
docker-compose.yml      Ugreen NAS deployment (nginx + cloudflared)
nginx.conf              Static-site server config
.env.example            Template for CLOUDFLARE_TUNNEL_TOKEN
```

## Deploy

One-time setup on the Ugreen NAS:

1. In Cloudflare Zero Trust → Networks → Tunnels, create a tunnel, copy
   the tunnel token.
2. In the tunnel's Public Hostname tab, configure:
   - Subdomain: (blank) or `www`
   - Domain: `iantrimble.com`
   - Service: `HTTP`
   - URL: `app-backend:80`
3. In the Ugreen App Center → Docker Compose, create a project rooted at
   `/volume1/nas/projects/app-website/`.
4. Copy `.env.example` to `.env`, paste your tunnel token into
   `CLOUDFLARE_TUNNEL_TOKEN=`.
5. Start the project. Expected healthy state: both containers "running";
   `curl -I https://iantrimble.com` returns `HTTP/2 200` with
   `server: cloudflare`.

## Day-to-day editing

Edit files in this directory on your Mac. Nginx serves directly from
the bind mount — changes are live on the site immediately. No deploy
step.

For Cloudflare caching, you may need to purge specific URLs after a
change: Cloudflare dashboard → Caching → Configuration → Purge by URL.
For instant preview: append `?v=<random>` to the URL.

## Hardening to-do (later)

- Swap the SVG-text logo for a rasterised `assets/og-1200x630.png`
  (currently falls back to a 404 for OG previews)
- Rasterise `assets/twitter-1200x675.png`
- Capture a real screenshot of Preview &amp; Edit view for the hero
  section (currently a stylised placeholder)
- Add `/changelog.html` and `/faq.html` (currently 404s in nav)
- Fill `press-kit/` with the `.icns`, a rasterised logo set, app
  screenshots at 1440×900 + 2880×1800, and a one-paragraph boilerplate

## Content editing cheatsheet

- Change pricing: `index.html` section `id="pricing"`.
- Add testimonial: currently absent by design (research advises not to
  fake). When real reviews land, replace the `#privacy-note` trust-grid
  block with a testimonial carousel.
- Add press logos: in the footer above the legal links, add an
  `<div class="press-rail">` with img tags.
