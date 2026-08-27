---
title: "Support"
description: "Common questions about why CalAround works the way it does, and a way to reach a human."
---

## Why do I have to review every scan?

Because a photograph of a calendar is a guess, and this app writes to something you rely on.

The reader is good at dense Outlook grids and bad at nothing in particular — but "bad at nothing in particular" is not the same as "never wrong". A blurry 8:30 that reads as 8:00, a title clipped by Outlook's own ellipsis, a block whose time is inferred from where it sits between gridlines: these are all normal, and all things you can spot in two seconds and the app cannot.

So CalAround shows the diff and waits. Anything it was unsure of is marked **Unverified** and sorted to the top, next to the region of the photo it came from. Tap a row to correct it before it lands.

## Why a separate "Work (Scanned)" calendar?

Because it makes the app's reach obvious and reversible.

CalAround writes only to that calendar and never touches any other. That single rule is what lets it safely *remove* an event later — it can only ever remove something it put there itself. It also means you can hide the whole thing with one toggle in Apple's Calendar app, share it separately, or delete it and be certain nothing of CalAround's is left behind.

The alternative — writing into your existing calendar and marking its own events somehow — would put CalAround's bookkeeping inside your data, and make a bug capable of deleting something it didn't create. Not worth it.

## What happens if a scan misreads an event?

Three things, in order of how early you can catch it:

1. **In review.** Tap the row, fix the time or title, and the corrected version is what syncs.
2. **After syncing.** History → *Undo this sync* replays the inverse: it deletes what was added, restores what was changed, and puts back what was removed. This works across app launches and days later.
3. **On the next scan.** If you fixed the event yourself in Apple's Calendar and want it left alone permanently, **pin** it. Pinned events are never modified or deleted by a scan; if a later scan disagrees, CalAround shows you the disagreement instead of overwriting your version.

## Why does it want full calendar access?

Write-only access lets an app add events but not read any back — including its own. With write-only, CalAround cannot tell an event it already wrote from one it hasn't, so every rescan would duplicate the week, and it could never remove or update anything.

Full access is what makes a *rescan* work rather than a re-add, and it's what lets conflicts be detected at all. If you grant write-only, the app still runs: it says so on screen and offers additions only.

## Why did my scan come back mostly "Unverified"?

Almost always the photo. Four things fix most of it:

- Fill the frame with the grid; crop out the sidebar and mini-calendar.
- Keep the hour labels down the left visible — that's how times get read.
- Straight on, no glare.
- If the week is scrolled, take two overlapping photos. Duplicates across them are merged, not double-added.

## Do I need an API key?

Only for the Claude reader, which is the one available today. You supply your own key from [Anthropic](https://console.anthropic.com), and it's stored in your iPhone's Keychain. Once the on-device Apple Intelligence reader ships with iOS 27, no key will be needed and it becomes the default.

You can try the whole app with no key at all: **Settings → Read photos with → Sample data (demo)**.

## Contact {#contact}

<div class="formwrap">
<!-- Netlify Forms. The three attributes below are the whole wiring: Netlify's
     post-processing parses the DEPLOYED html, finds the form by `name`, and
     starts accepting posts at this same path. `data-netlify` is the documented
     spelling of the bare `netlify` attribute and is valid html5. The hidden
     form-name input is what attributes a submission to this form — without it
     the post is accepted and filed nowhere. bot-field is the honeypot: real
     people never see it, bots fill it, and Netlify silently drops those. -->
<form name="contact" method="POST" action="/thanks/" data-netlify="true" netlify-honeypot="bot-field">
  <input type="hidden" name="form-name" value="contact">
  <p class="hp"><label>Leave this field empty <input name="bot-field" tabindex="-1" autocomplete="off"></label></p>
  <div class="field">
    <label for="cf-email">Your email</label>
    <input id="cf-email" type="email" name="email" required autocomplete="email">
  </div>
  <div class="field">
    <label for="cf-subject">Subject</label>
    <input id="cf-subject" type="text" name="subject" required>
  </div>
  <div class="field">
    <label for="cf-message">Message</label>
    <textarea id="cf-message" name="message" required></textarea>
  </div>
  <button class="btn btn-primary" type="submit">Send</button>
  <p class="formnote">Your address is used to reply to you and nothing else. It isn't added to a list, because there is no list.</p>
</form>
</div>
