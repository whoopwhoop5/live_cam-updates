# LiveCam 0.5.0

Released 2026-09-01.

First public release, and the first on the new native desktop shell.

### Added

- Native desktop application: one window, French throughout, with the
  dashboard, face, voice, and settings screens, an install and update flow,
  a guided device wizard, and a sealed support report.
- Virtual camera and virtual microphone installed by the setup program, so
  the result works in any meeting or streaming app.
- Instant voice: create a voice from a short reference recording and use it
  live within seconds, with an optional upgrade to a fully trained voice
  that keeps the same voice while it trains.
- Trained voices from short clips or long recordings, including speaker
  selection when a recording contains several people.
- Live face swap from a reference photo, with a fail-closed presence gate:
  the camera output stays black until a real face is confirmed and freezes
  on the last good frame when the face leaves.
- Live session protection: speech-only conversion (silence and room noise
  are never converted), onset protection so first words never stutter,
  automatic recovery when a microphone or speaker is unplugged or changed
  mid-session, and warnings for hands-free Bluetooth headsets and for a
  microphone that stopped delivering signal.
- One-time consent dialog before any voice audio leaves the machine.
- Self-service licence: activation, online validation, a 72 hour offline
  grace period, and deactivation from the settings screen.
- Hand-rolled updater with signed manifests, and a runtime installer that
  downloads and verifies the voice engine on first launch.

### Changed

- The whole desktop shell was rewritten natively; the previous web-based
  shell and its toolchain were removed.
- Real-time video (camera capture, cloud face leg, audio/video alignment,
  virtual camera writer) moved into a native worker process beside the app.
- The desktop shell is the single owner of the licence; the local service
  only verifies it.
- Lower default latency on capable machines, chosen automatically by a
  per-machine calibration with a safety margin.
- Noise suppression on the microphone input is on by default.

### Fixed

- Camera choice follows the physical device across USB renumbering instead
  of silently opening whatever sits at the saved index.
- A first-ever voice creation no longer shows "coming soon" while the local
  service warms up.
- The setup program's licence text renders correctly (encoding).

### Security

- Activations are bound to a server-issued secret and short-lived signed
  leases; the previous compatibility path was removed before release.
- Everything a user can see or download is vendor- and engine-neutral, and
  the release tree is scanned for secrets and protected details before
  publish.

---

Built from live_cam commit a9225d9. The installer above is the only file to download; the source archives are GitHub's automatic copy of this note.
