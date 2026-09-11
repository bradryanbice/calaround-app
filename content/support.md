---
title: "Support"
description: "Common questions about why CalAround works the way it does, and a way to reach a human."
---

## Why do I have to review every scan?

Because a photograph of a calendar is a guess, and this app writes to something you rely on.

The reader is good at dense Outlook grids and bad at nothing in particular — but "bad at nothing in particular" is not the same as "never wrong". A blurry 8:30 that reads as 8:00, a title clipped by Outlook's own ellipsis, a block whose time is inferred from where it sits between gridlines: these are all normal, and all things you can spot in two seconds and the app cannot.

So CalAround shows the diff and waits. Every meeting it would add, move, or remove is a row, and nothing is written until you tap **Sync**. Tap a row to correct it before it lands, or open the photo to check a read against the source.

## Which calendar should it write to?

By default CalAround creates its own calendar, **Work (Scanned)**, and writes there. That is the recommended choice, because it makes the app's reach obvious and reversible: everything on it came from a scan, so you can hide it with one toggle in Apple's Calendar app, share it separately, or delete it and be certain nothing of CalAround's is left behind.

You can point it at a calendar you already have instead, in **Settings → Calendar → Writes to**. Know what that means first: CalAround manages whichever calendar you give it. In a week you scan, it treats everything on that calendar as something to keep in step with the photo — **including events you added yourself** — so a scan can move them, or remove them if the photo doesn't show them. Settings keeps a warning on screen while such a calendar is selected. If you use one, pick a calendar that holds only your work meetings.

Either way, no other calendar is ever touched, nothing outside the scanned week is touched, and every change is listed for you to approve first.

## What happens if a scan misreads an event?

Three things, in order of how early you can catch it:

1. **In review.** Tap the row, fix the time or title, and the corrected version is what syncs.
2. **After syncing.** History → *Undo this sync* replays the inverse: it removes what was added, moves back what was moved, and re-creates what was removed. This works across app launches and days later. A re-created event comes back with its title and time, but not its invitees, alerts, or repeat schedule.
3. **On the next scan.** CalAround compares the photo with the calendar as it is now. If you fixed an event yourself in Apple's Calendar and the photo still disagrees, the next scan will offer to move it back — switch that row off to keep your version.

## Why does it want full calendar access?

Write-only access lets an app add events but not read any back — including its own. With write-only, CalAround couldn't tell a meeting already on your calendar from a new one, so every rescan would add the week again, and it could never move or remove anything.

Full access is what makes a *rescan* work rather than a re-add, so CalAround won't scan without it. It asks when you tap **Scan** — before your photo is sent anywhere — and if you decline, it says so and stops. iOS grants full access to every calendar at once, but CalAround reads events only from the one it writes to, plus your calendars' names so it can list them in Settings.

## Why did my scan miss or misread meetings?

Almost always the photo. Four things fix most of it:

- Fill the frame with the grid; crop out the sidebar and mini-calendar.
- Keep the hour labels down the left visible — that's how times get read.
- Straight on, no glare.
- If the week is scrolled, take two overlapping photos. Duplicates across them are merged, not double-added.

## Do I need an API key?

Yes, to scan a real photo. CalAround reads photos with Claude, from Anthropic, and you supply your own key from [Anthropic](https://console.anthropic.com). It's stored in your iPhone's Keychain, and any charges are between you and them.

Earlier test builds also offered an on-device reader using Apple Intelligence. It was removed because it couldn't read real photographs reliably, so Claude is now the only reader.

You can try the whole app with no key at all: **Settings → Read photos with → Sample data (demo)**. It still asks for calendar access, because syncing writes the demo week to your calendar like a real scan — undo it from History when you're done.

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
