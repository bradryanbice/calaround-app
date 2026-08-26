# calaround.app

The public site for [CalAround](https://github.com/bradryanbice/calaround), an iOS app that
photographs a work calendar, previews the diff, and syncs it to Apple Calendar. Live at
<https://calaround.app>.

**Hugo**, deployed to GitHub Pages by `.github/workflows/hugo.yml`. GitHub Pages only builds Jekyll
natively, so the workflow builds the site and publishes it as a Pages artifact — the repo's Pages
source is set to **GitHub Actions**, not a branch. Changing it back to a branch will break the deploy.

```
hugo server        # local preview
hugo --gc --minify # production build into public/
```

## Layout

| Page | Source | URL |
| --- | --- | --- |
| Landing | `layouts/index.html` | `/` |
| Support | `content/support.md` | `/support/` |
| Privacy Policy | `content/privacy.md` | `/privacy/` |
| Terms of Use | `content/terms.md` | `/terms/` |

The stylesheet lives in `assets/css/site.css` (Hugo's asset pipeline), **not** `static/` — it is
minified and **fingerprinted**, so the published filename carries a content hash. A changed
stylesheet is therefore a new URL and can never be served stale from cache. Images stay in
`static/` because they don't change.

`static/CNAME` holds the custom domain. `static/assets/img/` holds the app screenshots at the
iPhone 17 Pro's native 1206×2622, converted to WebP — deliberately **not** downscaled, because the
phone renders are large and crispness was the point.

## Before this goes live

- [ ] **Replace `formEndpoint` in `hugo.toml`** with a real Formspree form ID. The contact form on
      `/support/` posts to a placeholder and a visible TODO banner sits above it until you do. The
      banner is styled `.formtodo` — delete that `<p>` when the endpoint is real.
- [ ] **Have the legal pages reviewed.** `content/privacy.md` and `content/terms.md` are drafts
      written to describe what the app actually does, not lawyer-reviewed documents. The privacy
      page is accurate as of the audit below; the terms are a reasonable starting point, not advice.
- [ ] Point `calaround.app` DNS at GitHub Pages, and set Settings → Pages → Source to
      **GitHub Actions**.
- [ ] Add the site URL to the App Store Connect listing.

## The privacy page is not a template

Headed's site can say "no server, nothing leaves the device". CalAround's Claude reader **sends
your photo to Anthropic**, so that claim would be a lie here and the page was written from scratch
to say so plainly.

Its factual claims were verified against the app source, not assumed:

| Claim | How it was checked |
| --- | --- |
| One outbound endpoint | `grep` for every URL in the codebase → only `api.anthropic.com/v1/messages` |
| No analytics or tracking | `grep` for analytics/Firebase/Sentry/Segment/etc. → no hits |
| No third-party code | every `Package.swift` dependency is a local `path:` — no remote packages |
| Photos not persisted | scan history stores decoded events; the image is held in memory for the request only |
| Key in Keychain | `KeychainAPIKeyStore`, `kSecClassGenericPassword` |

**Re-run that audit before changing any privacy claim.** If the app gains a dependency, an endpoint,
or a crash reporter, this page is wrong until it is updated.

## Design

Direction: **"The Diff"** — the calendar grid is the ground the site sits on, and the features list
is laid out as a change gutter, with the same `+` / `~` / `−` marks and colours the app's review
screen uses. Palette derives from `CalAroundDesignSystem` (`Tokens/Palette.swift`): brand `#2450C8`
light / `#8FB0FF` dark, with the added/removed/modified triple carried across. Light and dark both
supported. Type is Archivo + DM Mono, shared with headedapp.com so the two sites read as siblings.

## Still to do

- Download / TestFlight link — blocked until an External TestFlight group exists. The hero carries
  a placeholder line saying the link lands there first.
- An App Store badge and screenshots sized for the listing, once there is a listing.
