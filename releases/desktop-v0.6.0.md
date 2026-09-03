# LiveCam 0.6.0

Released 2026-09-03.

The 2026-09-02 full-repository audit, executed the same day: every live
path (voice, face, go-live, licence, install and update) fixed where the
audit found it wrong, and the dead surfaces it found removed.

### Fixed

- Slow connections can now finish what they start: every large transfer
  used to carry a fixed total-time allowance that a slow but steadily
  progressing connection could never fit under, turning a bandwidth
  problem into a permanent failure (seen in the field at 92 KiB/s, where a
  finished voice's first file took 586 s of its 600 s allowance and the
  second could never fit, twice, with the voice vanishing on relaunch).
  Fixed across the four paths that had the pattern: installing a finished
  voice (the fixed per-file allowance is gone; the progress watchdog and
  file-size ceilings are the bounds, and the retry budget accommodates
  slow attempts), the one-time voice-engine download (its per-file
  allowance now scales with the bytes left to move, and hitting it retries
  with fresh links instead of abandoning the install, so partial progress
  is kept), restoring a voice library (each transfer is watched for real
  stalls instead of the whole library sharing one 25-minute clock that the
  first voices consumed), and the app-side wait on clone creation, clone
  upgrade and library import (bounds sized for the slowest honest
  transfer instead of a fast link's). Genuinely stalled transfers still
  fail fast; connections below about 0.5 Mbit/s remain unsupported by
  design.
- Live voice: one non-finite sample from the converter no longer poisons
  the output shaping for the rest of the session (the virtual microphone
  carried silence-as-not-a-number under a green health dot and the status
  route answered 500); the poisoned hop plays as silence and is counted.
- Live voice: the latency estimate is sampled once per hop instead of at a
  random phase of the output ring, so the automatic picture/voice alignment
  no longer retunes on that jitter.
- Live voice: after the machine's first speed measurement the GPU keepalive
  replays one graph per active stage instead of every captured shape, which
  had turned the silence keepalive into the onset stall it exists to remove.
- Live voice: at the low-latency rung the parent's hop follows the worker's
  block grid (one double-length hop every 24 and a permanent ring sawtooth
  are gone).
- Live voice: on capable machines the pre-warmed session is adopted at
  go-live instead of being rebuilt and then discarded for a mode mismatch
  that did not exist; one worker build per prewarm.
- Live voice: a loop stall drops the stale backlog instead of playing it
  late and chopped; a device reconnect fades the stale backlog out and
  restarts the converter on a silent context; the input capture zeroes a
  non-finite chunk before the converter sees it.
- Live face: the presence gate's freeze frame is pushed once, so the
  virtual camera's 10 s freeze cap engages instead of holding a still of
  the user for as long as they are away.
- Live face: the specific failure cause is no longer overwritten by the
  reconnect back-off a moment later, so the rail and support reports keep
  the real cause; presence inference runs on its own thread instead of
  stalling the output thread and dropping capture frames four times a
  second.
- Live face: an alive but unresponsive video worker degrades the face half
  (amber, then a French line) instead of staying "running" forever; the
  preview no longer re-opens its frame transport every 3 s while the
  output is deliberately held on a safe frame, and its torn-read guard no
  longer discards intact frames when the writer publishes once mid-copy.
- Go-live: a stop verdict that lands after a newer session started no
  longer toasts over the new session; licence deactivation and a
  mid-session licence rejection settle the live state machine; the live
  stop is one request per half instead of a three-attempt ladder.
- Voice clones: a job the service is cancelling reads as cancelling after a
  restart instead of as progress; storage faults and refused upgrades no
  longer leave voice-creation records pending (24 storage errors in a day
  could lock a seat out with abuse copy); a trained-voice install or library
  restore without the service's digests is refused and retried instead of
  installing unverified files; a deliberately deleted voice can no longer
  be resurrected by the startup recovery sweep from a stranded sibling
  directory; the long-recording cleanliness panel's music and background
  detectors fail closed on a missing reading like their siblings.
- Licence: a rolled-back clock is its own state, with copy naming the
  clock and the reconnect instead of "unlicensed" with a retry line.
- Install and update: a release that fails its post-install health check
  is held back for 24 h and the last known-good pair is started, instead
  of being re-downloaded and re-attempted on every launch; a failed silent
  self-update is narrated once by the next launch; the signed runtime
  channel manifest is re-signed by every backend release, so an
  app-code-only release cannot let it expire (a daily check alarms 30 days
  out); free space is checked before each download and extraction; a
  rolled-back setup wipe keeps the licence store; the expired-manifest
  case shows its own line instead of the connection-retry one.
- Installer: custom-directory installs lock every elevated-executed file;
  "also delete my data" deletes only the logged-on user's profile; the
  bundled audio driver payload is pinned file by file and
  signature-checked at build.
- App: a second launch from another logon session shows why nothing
  opens; a settings file quarantined at boot is narrated once; the shell
  pins the backend to loopback, and the backend refuses a non-loopback
  bind in a packaged build.

- First install and update on slower connections: the download link for the
  app code is now obtained right before that download starts, and obtained
  once more if the source refuses it mid-transfer, instead of being issued
  before the multi-gigabyte runtime download and expiring under it (a
  connection slower than about 50 Mbit/s hit "LiveCam ne peut pas vérifier
  ses fichiers" on every attempt). Download error text no longer carries the
  link's query string.
- Install screen truth: the bar keeps moving through the whole install. The
  app-code download after the runtime install is no longer discarded as a
  "rewind" (the bar sat still for that entire download), each finished
  runtime part shows its own verification progress instead of a frozen bar
  while it is hashed, a download whose source omits the length stays
  byte-true from the signed size, and the "connexion lente" line clears as
  soon as the transfer resumes rather than one chunk later.
- The install screen's final "Préparation terminée" line (and every other
  backend status line with an accent) no longer shows replacement
  characters: the backend's console wrapper took the Windows locale
  codepage instead of UTF-8, so every accented line on that pipe arrived
  as invalid bytes. The console is now UTF-8 explicitly, and status lines
  are additionally escaped to plain ASCII on the wire.
- The face path can no longer drain the cloud provider account on a dead
  camera input: a session that dies shortly after connecting now backs off
  (up to one reconnect per minute) instead of reconnecting every second, and
  session minting is capped at ten attempts per ten minutes.

### Changed

- Update channels: the moving pointers the app reads (the desktop feed and
  the runtime manifest) are committed files on the updates repository
  instead of release assets; the repository's immutable-releases policy had
  locked the channel releases. Installs of 0.5.0 read the frozen channels
  and must be reinstalled from the new setup.
- The shell's start and set-face payloads no longer carry the validation
  and privacy knobs the video worker had stopped reading; the live-status
  poll wakes on level-changing worker events only, not on the 2 s
  heartbeat.
- Release pipeline: dependency compiles are cached for the test and lint
  jobs (71 min cold to 7 min warm; the shipped build is still compiled from
  scratch), the desktop publish waits for a running security check instead
  of failing, and a forgotten version bump is refused before any build
  starts.

---

The installer above is the only file to download; the source archives are GitHub's automatic copy of this note.
