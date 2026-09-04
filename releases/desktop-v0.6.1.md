# LiveCam 0.6.1

Released 2026-09-03.

The interrupt-integrity release: every control that promises to stop
something now actually stops it, even when the work it interrupts is slow
or stuck, plus a faster delivery path for the big one-time downloads.

### Fixed

- Quitting during the first-run installation works: "Quitter et
  supprimer" now interrupts the download at its next moment instead of
  silently waiting for the whole transfer (on a slow connection the app
  could not be quit at all), and the clean-up-on-next-launch note is
  recorded only once the stop actually happened, so an abandoned quit can
  no longer silently erase a finished installation later.
- A stuck or slow backend update no longer locks the app: updating from
  the settings can be cancelled ("Annuler la mise à jour", the app stays
  on its current version), and quitting during any install download is
  immediate.
- Cancelling a voice creation now also stops the upload of the recording:
  before, the app's cancel only closed the panel while the transfer kept
  running to completion and the voice appeared later anyway, having spent
  its creation credit. An upload that trickles below the supported
  connection floor now ends with clear guidance instead of running for
  hours.
- Cancelling or deleting a voice wins against its own installation: a
  voice whose files were still downloading could reappear once the
  download finished, resurrecting what the user had deleted. The
  download now stops within moments and a finished installation checks
  for the cancellation before keeping anything.
- The voice engine download can be stopped: a new "Interrompre" control
  ends the transfer at its next piece, finished pieces are kept for the
  next resume, and the app no longer restarts the download by itself
  after the user stopped it.
- Importing a library can be cancelled while it runs (the cancel used to
  be greyed out for the whole operation), interrupting the transfers and
  leaving the library untouched; leaving the settings screen mid-import
  now also stops the import instead of letting it finish and apply
  invisibly.
- Closing the window while a session is still starting shows honest
  copy about a session in preparation, instead of claiming the user is
  already live.
- The backend's own shutdown is bounded instead of waiting forever on
  in-flight long requests.

### Changed

- The one-time voice-engine download is now served from the same fast
  delivery host as the application runtime instead of the file store,
  whose regional speed collapsed in the field (measured under 100 KiB/s
  while the delivery host ran at full speed on the same connection). The
  files remain encrypted end to end; the store remains the source and the
  fallback.

---

The installer above is the only file to download; the source archives are GitHub's automatic copy of this note.
