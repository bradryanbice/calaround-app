---
title: "Privacy Policy"
updated: "30 August 2026"
description: "CalAround has no account, no server of its own, and no analytics. The one thing that can leave your device is the photo you choose to scan — and only to the reader you pick."
---

CalAround photographs your work calendar and writes the meetings into a calendar on your iPhone. This page says exactly what that touches, what leaves the device, and what does not.

It is written plainly on purpose. If something here is unclear, [ask](/support/#contact) — a privacy policy nobody can follow isn't one.

## The short version

- There is **no CalAround account** and **no CalAround server**. Nothing is uploaded to us, because there is no us to upload to.
- There is **no analytics, no tracking, no advertising, and no third-party SDK** of any kind in the app.
- The **one** thing that can leave your device is a photo you explicitly choose to scan, and only when you have selected the Claude reader.
- Your calendar data stays on your iPhone and in your own iCloud, exactly as it did before you installed anything.

## The photo you scan

This is the part that deserves real care, because it depends on a choice you make.

CalAround offers more than one reader, named on the Scan screen and in Settings every time:

| Reader | What happens to the photo |
| --- | --- |
| **Claude** | The image is resized, compressed, and sent over HTTPS to Anthropic's API to be read. The extracted events come back; the image is not kept by CalAround afterwards. |
| **Apple Intelligence** | The photo is read entirely on your iPhone — the text recognition and the language model both run on the device. Nothing about the photo or the meetings leaves it. Requires a device with Apple Intelligence. |
| **Sample data** | No photo is read at all. A fixed demo week is returned so you can try the app. |

**With the Claude reader, your photo leaves your device.** That is worth stating flatly rather than burying: a photo of an Outlook week contains your meeting titles, and usually your colleagues' names and your employer's business. If that is not something you want to send to a third party, use a different reader, or don't scan that week.

Anthropic processes the image under **their** terms and privacy policy, not ours — see [anthropic.com/privacy](https://www.anthropic.com/privacy). Because you supply your own API key, that request is made under your own account with them, and their terms govern it directly. We make no claims on their behalf about retention.

CalAround itself does not store the photograph. It is held in memory for the length of the scan and discarded. What is saved to your device is the *result*: the events that were read.

### Your API key

If you use the Claude reader, you paste your own Anthropic API key into Settings. It is stored in the **iOS Keychain** on that device — and only that device: the key is excluded from backups and device transfers, so moving to a new iPhone means pasting it again. It is never written to a file, never logged, never bundled into the app, and never sent anywhere except to Anthropic's API in the request header. Remove it in Settings at any time.

## Your calendar

CalAround asks for calendar access, and the level it asks for changes what it can do:

| Access | What it allows |
| --- | --- |
| **Write-only** | Adding events. iOS does not permit reading events back with this level — including the app's own — so change detection and conflict checks are impossible, and CalAround can only add. |
| **Full** | Reading back what it previously wrote, so a rescan can update or remove instead of duplicating, and so clashes with your other calendars can be flagged. |

Whichever you grant, the same limits hold:

- CalAround writes to **one calendar at a time — the one you choose**. By default that is a calendar it creates, named **Work (Scanned)**, where a rescan can add, update, and remove its own events. If you point it at a calendar you already had, it **only ever adds** — it cannot tell its events from yours there, so it never modifies or deletes anything on a calendar it didn't create. Events on every other calendar are untouched, ever.
- It only acts within the **date range the scanned photo actually showed**. Events outside that range are untouchable.
- Conflict detection **reads** your other calendars to compare times. It does not copy, upload, or store their contents — a clash is evaluated and shown, and the comparison is discarded.

You can revoke access at any time in **Settings › Privacy & Security › Calendars**, and delete the *Work (Scanned)* calendar in Apple's Calendar app to remove everything CalAround ever wrote.

## Watching your personal calendar

This feature is **off by default and opt-in.** When enabled, CalAround re-checks for clashes when your calendars change and when you open the app, and can post a local notification when a new one appears.

- It requires **full** calendar access and notification permission. You will be asked for both.
- Notifications are **local** — generated on your iPhone by iOS. Nothing is sent to a server to produce them.
- The notification text **names the two clashing meetings**. Whether that text is visible on your lock screen is iOS's preview setting — **Settings › Notifications › Show Previews**, which hides it until unlock by default on Face ID devices.
- iOS decides when to wake a background app, so a notification can arrive minutes — occasionally hours — after the change. Opening CalAround always checks immediately.

## What is stored on your device

- **Scan history.** The last 20 scans: the events read, when the scan happened, which reader produced it, and what each sync changed. This is what makes *Undo this sync* possible. It never includes the photograph.
- **Preferences.** Your chosen reader, conflict threshold, title-privacy rules, and which events you have pinned.
- **Your API key**, in the Keychain, if you added one.

All of it lives in the app's own container and is removed when you delete the app. Scan history and preferences are included in an encrypted iPhone backup if you make one, the same as any other app's data; the API key is not — it never leaves the device it was pasted on.

## Title privacy

Three things happen to meeting titles *before* anything is written to your calendar. All of them run on your device, and what you review in the app is exactly what will land.

- **Join links are always stripped.** Zoom, Teams, and Meet links, dial-in numbers, meeting IDs, and passcodes are removed from titles on every scan. This is not a setting — that joining junk is never stored, full stop.
- **People's names become initials, by default.** *1:1 with Marta Chen* lands as *1:1 with M.C.* Names are detected on your iPhone using Apple's on-device text analysis — the detection sends nothing anywhere — and every title this touches is labelled **Name hidden** on the review screen, so nothing is rewritten behind your back. Turn it off in Settings if you'd rather keep names.
- **Your own rules run first.** A rule like *title contains "1:1" → write "Busy"* replaces the whole title, and the first matching rule wins. A rule's replacement is used as you wrote it — the name-hiding pass doesn't second-guess words you chose on purpose.

This is a convenience, not a security boundary: it changes what is written to your calendar, not what was in the photo you scanned.

## Children

CalAround is not directed at children and does not knowingly collect information from anyone. There is nothing to collect.

## Changes to this policy

If what the app does changes, this page changes with it, and the date at the top moves. Material changes — particularly anything altering what leaves your device — will be called out in the app, not just here.

## Contact

Questions about any of this: [send a message](/support/#contact).
