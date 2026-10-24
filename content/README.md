# Content updates

This directory holds the signed content patch the app fetches.

Two files, one per edition:

    patch.json             public App Store edition
    patch-plantcity.json   Plant City PD agency edition

Both are produced by `npm run patch -- draft.json` in the app repo, which
writes `draft.signed.json`. Rename and put it here.

They are separate so the two products can be corrected on different schedules.
A department may want a change seen by its legal advisor before it reaches
their officers, and the agency build carries local ordinances the public patch
has no business touching.

## Why there is nothing here yet

The app treats an absent file as "no update published", which is the correct
state before the first correction. It is not an error, and the app runs on the
content it shipped with. Nothing needs to exist here for the app to work.

## Publishing a correction

1. Write a draft in the app repo (see `tools/make-patch.js` for the shape).
2. `npm run patch -- draft.json` — this refuses the draft locally if the app
   would refuse it on a device: wrong version number, an undocumented change,
   too many removals.
3. Copy `draft.signed.json` here as `patch.json`.
4. Commit and push. GitHub Pages serves it within a minute or two.

Devices pick it up on their next launch. Content deliberately does not change
mid-session — a holding must not rewrite itself under an officer who is in the
middle of reading it.

## This directory is public, and that is fine

The patch is Ed25519-signed and the app verifies it against a key compiled into
the binary before parsing anything. A patch served from a compromised host, or
from somewhere else entirely, is discarded. The host is a convenience, not a
trust boundary.

What this means in practice: do not worry about who can read this file. Do
worry about `tools/private-key.txt` in the app repo, which signs it. That file
is gitignored and must never be committed, pasted into a chat, or copied onto a
machine you do not control. Losing control of it is the only way this mechanism
fails.

## Version numbers

`version` must increase with every patch. The app refuses anything at or below
the version already applied, which is what stops an old patch being replayed to
undo a correction. Keep a note of the last one you published — the app's
Content & Updates screen shows what a device currently has applied.
