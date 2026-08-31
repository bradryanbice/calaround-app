# calaround.app

The public site for [CalAround](https://github.com/bradryanbice/calaround), an iOS app that
photographs a work calendar, previews the diff, and syncs it to Apple Calendar. Live at
<https://calaround.app>.

**Hugo**, deployed by **Netlify** from `netlify.toml` — the same host as bradbice.com,
playoffsbracket.com and royalrumblestats.com. Build settings live in that file, not in the Netlify
UI: build command, publish directory, `HUGO_VERSION`, redirects and headers are all committed here,
so changing them in the dashboard would be overwritten on the next deploy.

`netlify.toml` mirrors royalrumblestats.com, which is the closest sibling — same Hugo version, and
no dart-sass step, because this site's one stylesheet is plain CSS through Hugo's own pipeline
rather than SCSS.

The old GitHub Pages deploy (`.github/workflows/hugo.yml`, plus `static/CNAME`) is **still in place
on purpose** — see the checklist below. It comes out once calaround.app is confirmed serving from
Netlify, not before.

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

`static/CNAME` holds the custom domain **for GitHub Pages only** — Netlify takes the domain from
its own settings and ignores this file. `static/assets/img/` holds the app screenshots at the
iPhone 17 Pro's native 1206×2622, converted to WebP — deliberately **not** downscaled, because the
phone renders are large and crispness was the point.

## The contact form

`/support/` posts to **Netlify Forms**. There is no third-party form service and no endpoint to
configure — the wiring is three attributes on the `<form>` in `content/support.md`:

| Piece | Why it is there |
| --- | --- |
| `name="contact"` + `data-netlify="true"` | Netlify's post-processing parses the **deployed** HTML, finds the form by name, and starts accepting posts. `data-netlify` is the documented spelling of the bare `netlify` attribute, and is valid HTML5. |
| `<input type="hidden" name="form-name" value="contact">` | Attributes the submission to that form. Without it the POST is accepted and filed nowhere. |
| `netlify-honeypot="bot-field"` + the `.hp` field | Spam trap. Hidden from people, filled by bots, silently dropped. |

`action="/thanks/"` sends a successful submission to `content/thanks.md` instead of Netlify's
generic success page. That page is `build.list: never`, so it stays out of the sitemap and every
page list.

Because detection happens at **deploy** time against the built HTML, the form does nothing on
`hugo server` locally and nothing on a branch that hasn't deployed. Test it on a deploy preview or
the live site, not locally.

## Before this goes live

- [ ] **Finish the Netlify migration.** Repo-side config is committed; the rest is dashboard and
      DNS work — see [issue #1](https://github.com/bradryanbice/calaround-app/issues/1) for the
      full checklist. In short: connect the repo as a new site, confirm it renders on the
      `*.netlify.app` subdomain, add `calaround.app` as a custom domain, move DNS at Namecheap,
      confirm the Let's Encrypt cert provisions.
- [ ] **Turn on form notifications.** Netlify → Forms → *contact* → notifications → email. Then
      submit the live form once and confirm it lands in the dashboard **and** in the inbox. A form
      that collects silently is worse than no form.
- [ ] **Have the legal pages reviewed.** `content/privacy.md` and `content/terms.md` are drafts
      written to describe what the app actually does, not lawyer-reviewed documents. The privacy
      page is accurate as of the audit below; the terms are a reasonable starting point, not advice.
- [ ] **Decommission GitHub Pages** once calaround.app serves from Netlify: delete
      `.github/workflows/hugo.yml` and `static/CNAME`, and set Settings → Pages → Source to None.
      Do this last — while both exist, Pages keeps building harmlessly, and it is the fallback if
      the DNS move needs backing out.
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
| On-device reader is the default | `AppSettings` defaults `providerID` to `"apple-fm"`; `AppleFMProvider` heads `CalAroundApp`'s `providers` array (2026-08-31) |
| Name hiding warns before it is switched off | `SettingsView` — the toggle's setter raises an alert instead of applying `false`, and a caution row persists while it is off (2026-08-31) |

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
