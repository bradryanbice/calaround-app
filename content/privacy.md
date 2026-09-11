---
title: "Privacy Policy"
updated: "10 September 2026"
description: "CalAround has no account, no server of its own, and no analytics. The one thing that leaves your device is the photo you choose to scan, sent to Anthropic to be read."
---

CalAround photographs your work calendar and writes the meetings into a calendar on your iPhone. This page says exactly what that touches, what leaves the device, and what does not.

It is written plainly on purpose. If something here is unclear, [ask](/support/#contact) — a privacy policy nobody can follow isn't one.

## The short version

- There is **no CalAround account** and **no CalAround server**. Nothing is uploaded to us, because there is no us to upload to.
- There is **no analytics, no tracking, no advertising, and no third-party SDK** of any kind in the app.
- **The photo you scan is sent to Anthropic to be read.** That is the one thing that ever leaves your device, and only when you tap Scan.
- CalAround edits **one calendar — the one you choose** — and only within the week you scanned. If you choose a calendar you already had, that can include **moving or removing events that were already on it**.
- Your calendar data stays on your iPhone and in your own iCloud, exactly as it did before you installed anything.

## The photo you scan

CalAround has one reader that reads photos, and a demo that doesn't. Whichever is selected is named on the Scan screen and in Settings.

| Reader | What happens to the photo |
| --- | --- |
| **Claude** *(the reader)* | The image is resized so its longest edge is at most 1,568 pixels, compressed as a JPEG, and sent over HTTPS to Anthropic's API to be read. The extracted events come back; the image is not kept by CalAround afterwards. Along with the image, the request carries today's date and your time zone, so relative labels like "Mon 25" can be read — nothing from your calendar is sent. |
| **Sample data** | No photo is read and nothing is sent. A fixed demo week is returned so you can try the app; syncing it writes those demo events to your calendar like any other scan. |

**Your photo leaves your device.** That is worth stating flatly rather than burying: a photo of an Outlook week contains your meeting titles, and usually your colleagues' names and your employer's business. If that is not something you want to send to a third party, don't scan that week.

Anthropic processes the image under **their** terms and privacy policy, not ours — see [anthropic.com/privacy](https://www.anthropic.com/privacy). Because you supply your own API key, that request is made under your own account with them, and their terms govern it directly. We make no claims on their behalf about retention.

CalAround itself does not store the photograph. It is held in memory while you scan and review, and discarded. What is saved to your device is the *result*: the events that were read.

### Your API key

**Scanning a photo needs your own Anthropic API key; the demo doesn't.** You paste the key into Settings. It is stored in the **iOS Keychain** on that device — and tied to that device: it does not move to a new iPhone through a backup or device transfer, so moving to a new iPhone means pasting it again. It is never written to a file, never logged, never bundled into the app, and never sent anywhere except to Anthropic's API in the request header. Remove it in Settings at any time.

## Your calendar

CalAround needs **full** calendar access to scan, and asks for it when you tap Scan — before your photo is sent anywhere. Full access is what lets it see what is already on the calendar you choose, so a rescan can move and remove events instead of adding the week again. If you decline, or have granted only write-only access, scanning stops and the app tells you why; it does not fall back to adding blindly.

iOS grants full access to every calendar at once. CalAround reads events from **only one of them** — the one it writes to — plus the names of your calendars, so it can list them in Settings. Nothing it reads is uploaded.

What it does with that calendar:

- CalAround writes to **one calendar at a time — the one you choose**. By default that is a calendar it creates, named **Work (Scanned)**. You can choose a calendar you already had instead. Events on every other calendar are untouched, ever.
- **On that calendar, a scan adds, moves, and removes.** It treats every event on it within the scanned week as something to keep in step with the photo — **including events you added yourself.**
- **So if you choose a calendar you already had, CalAround may move or remove events that were already on it, within the scanned week.** An event of yours that the photo doesn't show will be listed for removal; one with the same title as a meeting in the photo may be listed to move to that meeting's time. Settings shows a warning for as long as such a calendar is selected. Pick a calendar that holds only your work meetings.
- Nothing is moved or removed without a row you approved on the review screen. A repeating event is changed one occurrence at a time, never as a whole series.
- It only acts within the **date range the scanned photo actually showed**. Events outside that range are untouchable.
- Each event CalAround writes or moves carries a short code in its URL field so the next scan can recognise it even after name hiding has changed its title. The code is a hash, not the readable title. A link you put on an event yourself is never replaced.

**Undo this sync**, in History, reverses a sync: it removes what was added, moves back what was moved, and re-creates what was removed. A re-created event comes back with its title, time, and all-day setting — **not its invitees, alerts, notes, or repeat schedule**. That matters most on a calendar you already had, where a removed event may have carried all of those.

You can revoke access at any time in **Settings › Privacy & Security › Calendars**. If CalAround wrote to its own *Work (Scanned)* calendar, deleting that calendar in Apple's Calendar app removes everything it ever wrote.

## Notifications and background activity

**None.** CalAround does not ask to send notifications, posts none, and does not run in the background. It reads and writes your calendar only while you are using it.

## What is stored on your device

- **Scan history.** The last 20 scans: the events read, the date range, when the scan happened, which reader produced it, and what each sync changed. This is what makes *Undo this sync* possible. It never includes the photograph.
- **Preferences.** Your chosen reader, which calendar to write to, your title-privacy rules, whether name hiding is on, and whether you have seen the introduction.
- **Your API key**, in the Keychain, if you added one.

All of it lives in the app's own container and is removed when you delete the app. Scan history and preferences are included in an encrypted iPhone backup if you make one, the same as any other app's data; the API key is stored so that it can only ever be restored to the iPhone it was pasted on.

## Title privacy

Three things happen to meeting titles *before* anything is written to your calendar. All of them run on your device, and what you review in the app is exactly what will land.

- **Join links are always stripped.** Zoom, Teams, and Meet links, dial-in numbers, meeting IDs, and passcodes are removed from titles on every scan. This is not a setting — that joining junk is never stored, full stop.
- **People's names become initials, by default.** *1:1 with Marta Chen* lands as *1:1 with M.C.* Names are detected on your iPhone using Apple's on-device text analysis — the detection sends nothing anywhere — and every title this touches is labelled **Name hidden** on the review screen, so nothing is rewritten behind your back.

  Name hiding happens after the photo is read, so it changes what is written to your calendar — the photo sent to Anthropic still shows names as they appear on your screen.

  You can turn it off in Settings if you'd rather keep names, but the app will ask you to confirm first and tell you what changes: names get written to your calendar in full, where anyone you share that calendar with can read them, and where they show on your lock screen unless you have previews hidden. While it stays off, Settings keeps showing a reminder that it is off. Your choice is remembered until you change it back.

  **If name hiding can't run, CalAround says so rather than pretending.** The detection relies on a language model that iOS downloads on demand, and on a device where it isn't available yet the app checks, finds it can't hide names, and puts a warning at the top of the review screen saying names will be written in full. It does not quietly write full names under a setting that claims otherwise — a silent failure here would be indistinguishable from a week that simply had no names in it, which is exactly why the app tests the detector before trusting it.
- **Your own rules run first.** A rule like *title contains "1:1" → write "Busy"* replaces the whole title, and the first matching rule wins. A rule's replacement is used as you wrote it — the name-hiding pass doesn't second-guess words you chose on purpose.

This is a convenience, not a security boundary: it changes what is written to your calendar, not what was in the photo you scanned. What the review screen shows you is always what will land — including when a pass didn't run.

## Children

CalAround is not directed at children and does not knowingly collect information from anyone. There is nothing to collect.

## Changes to this policy

If what the app does changes, this page changes with it, and the date at the top moves. Material changes — particularly anything altering what leaves your device — will be called out in the app, not just here.

**10 September 2026.** The on-device Apple Intelligence reader was removed, so every real scan now sends the photo to Anthropic. Removed alongside it: conflict detection, the opt-in watcher that re-checked your personal calendar and posted clash notifications, pinned events, and the rule that CalAround only ever *added* to a calendar it didn't create. It now adds, moves, and removes on whichever calendar you choose, and requires full calendar access to scan.

## Contact

Questions about any of this: [send a message](/support/#contact).
