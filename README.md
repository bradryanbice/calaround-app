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

**Netlify is the only deploy path.** The old GitHub Pages deploy
(`.github/workflows/hugo.yml`, plus `static/CNAME`) was removed on 2026-08-31 once calaround.app
was confirmed serving from Netlify, and the Pages site itself is deleted. There is no fallback now:
backing the DNS out would mean restoring that workflow from git history first.

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

`static/assets/img/` holds the app screenshots at the
iPhone 17 Pro's native 1206×2622, converted to WebP — deliberately **not** downscaled, because the
phone renders are large and crispness was the point.

**All seven were recaptured on 2026-09-11** from the simplified app (calaround `main` at
`4a87f12`), because every original showed something the 2026-09-10 simplification removed —
conflicts, tight-fit warnings, confidence chips, the watcher's settings. They are real captures of
the running app, not mockups, and the diff in them is a real diff:

- **Device** — the "CalAround Verify" simulator (iPhone 17 Pro), light appearance, default text
  size, status bar overridden to 9:41 with full battery: `xcrun simctl status_bar <udid> override
  --time 9:41 --batteryState discharging --batteryLevel 100 --cellularMode notSupported`.
- **Reader** — Sample data (demo), with full calendar access pre-granted
  (`xcrun simctl privacy <udid> grant calendar com.bradbice.calaround`).
- **The diff** — scan once and sync, after editing *Roadmap workshop* to 3:00 PM in review. Then,
  in the simulator's Calendar app, delete *Release go/no-go* and add *Budget review* (Tue 2:30 PM)
  to *Work (Scanned)*. The second scan shows 1 added, 1 moved, 1 removed. The removal has to come
  from Calendar rather than from a review edit: an edited row keeps its original sync key, so it
  would pair back up and show as a move.
- **The photo in `06`** — the Sample week drawn by `CalAroundEval`'s `OutlookRenderer`, so the
  photo and the parsed list genuinely match. Added to the simulator with `xcrun simctl addmedia`.
- **Encoding** — `xcrun simctl io <udid> screenshot`, then `cwebp -q 90 -m 6`.

`04-week-grid` is not used on the page since the conflicts section was removed; it is kept current
in case it comes back.

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
- [x] **Turn on form notifications** — done 2026-08-31, confirmed by a live test submission coming
      through. The form works end to end on the real domain. Worth remembering why this mattered:
      Netlify detects a form at *deploy* time from the built HTML, so it did nothing at all while
      GitHub Pages served the site, and it started genuinely accepting posts the moment DNS moved.
      There was a window where submissions would have filed silently.
- [ ] **Have the legal pages reviewed.** `content/privacy.md` and `content/terms.md` are drafts
      written to describe what the app actually does, not lawyer-reviewed documents. The privacy
      page is accurate as of the audit below; the terms are a reasonable starting point, not advice.
- [x] **Decommission GitHub Pages** — done 2026-08-31, once calaround.app was confirmed serving
      from Netlify (`server: Netlify`, apex on their ALIAS addresses, `www` CNAME'd to
      `calaround-app.netlify.app`). `.github/workflows/hugo.yml` and `static/CNAME` are gone and
      the Pages site is deleted. Netlify is now the only deploy path, and there is no fallback:
      backing the DNS out would mean restoring the workflow from git history first.
      `static/CNAME` mattered more than it looked — it made **GitHub Pages claim `calaround.app`
      as its own custom domain**, so two services held the same name and only DNS decided which
      answered. That claim is now released.
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
| Photos not persisted | `ScanHistoryEntry` stores decoded events and a changeset, no image data; no `FileManager`, `PHPhotoLibrary`, or `UIImageWriteToSavedPhotosAlbum` anywhere in the app |
| What the request carries | `ImagePayload` — longest edge 1568 px, JPEG 0.85; `ClaudeProvider` adds only `ExtractionHints`' reference date and time zone, nothing from the calendar |
| Key in Keychain, tied to the device | `KeychainAPIKeyStore`, `kSecClassGenericPassword`, `kSecAttrAccessibleAfterFirstUnlockThisDeviceOnly` |
| Claude is the only reader that reads a photo | `CalAroundApp`'s `providers` array is `ClaudeProvider` then `SampleProvider`; `AppleFMProvider` is deleted; `AppSettings` defaults `providerID` to `"claude"`, and a device still holding `"apple-fm"` falls back to the first entry (2026-09-10) |
| No notifications, no background work | `grep` for `UserNotifications` / `BGTaskScheduler` over the app and every package it links → none; `Info.plist` has no `UIBackgroundModes` or `BGTaskSchedulerPermittedIdentifiers`; `CalAroundConflict` is not linked by the app target (2026-09-10) |
| Full access required, asked before the upload | `ScanCoordinator.scan()` requests `.full` before calling the provider and throws `calendarAccessDenied` without it; no caller requests write-only (2026-09-10) |
| Reads events from the target calendar only | every `predicateForEvents` in `EventKitCalendarStore` passes `calendars: [calendar]`; `calendars(for:)` is used only for names in the picker |
| May move or remove existing events on a calendar the user already had | `ownedEvents(in:)` returns every event on the target in range; `apply` modifies and deletes there whether or not the app created it; `SettingsView` shows a warning while `isAppOwned == false` (2026-09-10) |
| One occurrence at a time | `save` / `remove` use `span: .thisEvent` |
| Sync key on the event's `url`, never over a user's link | `EventKitCalendarStore.apply(_:to:)` writes the key only when `url` is empty or already a key; `SyncKey` stores a hash, not the title |
| Undo re-creates without invitees, alerts, notes, or recurrence | `EventPayload` holds only title, start, end, all-day, and key; `revert` re-creates a deleted event from it |
| Name hiding warns before it is switched off | `SettingsView` — the toggle's setter raises an alert instead of applying `false`, and a caution row persists while it is off (2026-08-31) |
| Name hiding warns when it cannot run | `NameRedactor.prepare()` proves the tagger finds a name before reporting ready; `ScanCoordinator` skips redaction on `.unavailable` and `ChangeReviewView` shows the warning (calaround#15, 2026-08-31) |

Last re-run **2026-09-10**, against the simplification on calaround's `simplify/full-edit-calendar`,
which removed the on-device reader, conflicts and the watcher, pins, and add-only mode.

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
