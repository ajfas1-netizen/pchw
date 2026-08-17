# palmcityhandwash.com

Static site for **Striking Details** (dba Palm City Hand Wash). Deploys to GitHub Pages from
`ajfas1-netizen/pchw`. No build step, no dependencies, no framework.

```
index.html            the whole site
404.html              redirects to home
CNAME                 palmcityhandwash.com
.nojekyll             stops GitHub running Jekyll over the files
robots.txt
sitemap.xml
assets/
  logo.png            hero logo, 1200px wide, transparent
  logo-wordmark.png   sticky header lockup
  og-image.png        1200x630 social share card
  icon-32/180/512.png favicon and apple touch icon
```

---

## 1. Push the files

```bash
git clone https://github.com/ajfas1-netizen/pchw.git
cd pchw
# copy the contents of this package into the repo root
git add -A
git commit -m "Launch palmcityhandwash.com"
git push origin main
```

`.nojekyll` matters. Without it GitHub ignores any file or folder starting with an underscore.

## 2. Turn on Pages

Repo → **Settings → Pages**
- Source: **Deploy from a branch**
- Branch: **main**, folder: **/ (root)**
- Save

You will get `https://ajfas1-netizen.github.io/pchw/` within a minute or two. Confirm it works
before touching DNS.

## 3. Point the domain

**Add the custom domain in GitHub first.** Settings → Pages → Custom domain →
`palmcityhandwash.com` → Save. Then add these records in Cloudflare DNS:

| Type | Name | Value | Proxy |
|---|---|---|---|
| A | `@` | `185.199.108.153` | **DNS only** |
| A | `@` | `185.199.109.153` | **DNS only** |
| A | `@` | `185.199.110.153` | **DNS only** |
| A | `@` | `185.199.111.153` | **DNS only** |
| CNAME | `www` | `ajfas1-netizen.github.io` | **DNS only** |

Optional IPv6, same `@` name, also DNS only:
`2606:50c0:8000::153`, `2606:50c0:8001::153`, `2606:50c0:8002::153`, `2606:50c0:8003::153`

**Set the proxy to DNS only, the grey cloud, not the orange one.** With Cloudflare proxying
turned on, GitHub cannot complete the Let's Encrypt challenge and you never get a certificate.
Once GitHub shows the domain as verified and **Enforce HTTPS** is available, tick it. You can
switch the proxy back on afterwards if you want Cloudflare caching, but only with SSL mode set
to **Full (strict)**. Leaving it on Flexible will cause a redirect loop.

DNS can take up to 24 hours, though Cloudflare is usually minutes.

## 4. Connect the form  ← the one thing that is not done

GitHub Pages serves static files only. It cannot receive a form post. The form in `index.html`
has `action="__FORM_ENDPOINT__"` as a placeholder and will show an alert until you replace it.

Recommended: [Formspree](https://formspree.io) free tier. Create a form, copy the endpoint, then
in `index.html` replace `__FORM_ENDPOINT__` with e.g. `https://formspree.io/f/xxxxxxxx`.
Alternatives that work the same way: Basin, Formsubmit.co, Getform.

**On photos.** Free form services do not accept file uploads, and Nick quotes off condition, so
photos matter. That is why the page does not have an upload field. Instead there is a
**Text photos** button that opens the customer's SMS app pre-addressed to Nick with the message
started. On a phone that is fewer taps than a web upload and the photos land somewhere Nick
already looks. Revisit real uploads only if the volume justifies a paid tier or a Cloudflare
Worker plus R2.

## 5. Before you send anyone here

- [ ] Replace the form endpoint (step 4)
- [ ] Confirm every price in `index.html` against Nick's actual costs. They are calibrated to the
      local market, not to his margins.
- [ ] Confirm the warranty durations. The 3, 5 and 9 year tiers are placeholders shaped to the market.
- [ ] Add "Licensed and Insured" to the trust row **only once the policy is bound**. There is a
      fourth box currently reading "Owner Operated" that can be swapped for it.
- [ ] **Create the Google Business Profile.** For a local mobile service business this outperforms
      the website for lead volume. The site supports it, it does not replace it.

## Editing

Everything is in `index.html`. Prices live in the `CAR` and `BOAT` arrays in the script block near
the bottom. The boat calculator and the wax versus ceramic maths derive from `BOAT`, so changing a
per-foot rate updates the comparison automatically.

## Notes

- The logo is a raster PNG. Fine at every size used here. A vector redraw is required before any
  large format work such as a vehicle wrap.
- Structured data is `AutoDetailing` LocalBusiness JSON-LD with `areaServed` covering Palm City,
  Stuart, Jensen Beach, Hobe Sound, Port St. Lucie, Jupiter and Martin County. Keep it accurate,
  it feeds local search results.
