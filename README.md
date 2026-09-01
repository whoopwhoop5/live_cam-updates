# LiveCam updates

Public, signed release artifacts for the LiveCam desktop app. The app's
updater and runtime installer read this repository; people rarely need to.

## How to find a version

Use the **Releases** page, not Tags. Each entry is one of:

| Entry | What it is | Download it? |
|---|---|---|
| `desktop-vX.Y.Z` (titled *LiveCam X.Y.Z*) | The desktop installer for that version, signed, immutable. Its notes are the release notes. | Yes: this is what a person installs. |
| `backend-runtime-vDATE.N` (titled *Backend runtime …*) | The voice runtime bundle the app downloads and verifies on first launch. Immutable. | No: the app installs it. |
| `desktop-latest`, `backend-runtime-latest` (titled *channel pointer*) | Moving pointers the app reads (`latest.json`, signed runtime manifest). Their notes say which version they currently name. | No. |

The newest desktop version is marked **Latest** on the Releases page.

## Release notes

Every release's notes are in its GitHub release body and in
[`releases/`](releases/) (one file per tag). Notes are written in English
for the people who build LiveCam; the app shows its own French line.

## About the "Source code" archives

GitHub attaches `Source code (zip / tar.gz)` to every release automatically;
that cannot be turned off. Each tag points at the commit that added its
notes file, so those archives contain nothing but this README and the
notes. Ignore them.
