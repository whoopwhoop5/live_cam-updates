# LiveCam 0.7.0

Released 2026-09-07.

The redesign and audit release: every screen is rebuilt on one visual
language, and the September full-repository audit's findings are closed,
from a voice that dropped out on the first word after a silence to a
camera check that sent people to the installer when it had simply failed
to look.

### Added

- A new look across the whole application: refreshed light and dark
  appearances, one consistent depth for cards, floating controls and
  dialogs, the header tabs on a segmented bar, and one shared set of
  controls so every screen behaves the same way. Notifications stack with
  the newest in front and pause their timers while hovered; dialogs keep
  the keyboard inside them; long device and voice names shorten in place
  and show the full name on hover.
- "Arrêt à confirmer": when a stop cannot be confirmed, the app says so
  instead of claiming the session ended. The status chip, the dashboard
  headline and the close dialog tell the same story, and the session stays
  owned until an explicit stop succeeds.
- Live face says when the picture is being held ("Vidéo en pause", with
  what to do about it), so a frozen image reads as the protection working
  instead of as a broken camera.
- A library import can be cancelled while it runs, with an honest result:
  "Import annulé. Votre bibliothèque n'a pas été modifiée." before the
  change is applied, "Import terminé" when the cancellation came too late.
- "Réessayer" on the camera check: when the check itself fails, the
  pre-live list and the dashboard offer a retry with guidance instead of
  the installer button.
- The voice page keeps a small stop control visible even when the rest of
  the page is hidden, so a running preview can always be stopped.
- A notice at go-live when the automatic pitch adjustment could not be set
  because the microphone check never measured your voice ("La hauteur
  automatique n'a pas pu être réglée. Refaites la vérification du micro
  dans Micro et sortie."). The session still starts.
- Voice creation tells you when its progress has stopped being refreshed
  ("Suivi interrompu"), with the reason and, when the licence is the cause,
  a "Vérifier la licence" action. Progress freezes instead of showing a
  stale percentage as if it were fresh, and the creation itself is kept.
- A "Vérifiez votre licence" notice when the licence needs to be verified
  again online, alongside the existing expiry notices.
- The device wizard's last step names exactly what to pick in the call
  application, how to confirm it, and now also recommends a headset or
  earphones to avoid echo: the transformed voice reaches the call about a
  second late, and on laptop speakers that delayed far end feeds back
  through the microphone.

### Changed

- The instant-mode readiness badge no longer says "Bientôt disponible"
  while it is still checking: it says "Vérification…" during the check and
  "Momentanément indisponible" when the check could not conclude.
- The output list recommends only the audio cable that ships with LiveCam
  under "Recommandé pour vos appels"; other virtual outputs move to
  "Autres sorties", because they can need a separate bridge application.
- The microphone to pick in the call application is named the same way in
  the wizard and on the dashboard, instead of two screens naming two
  different devices.
- The wizard's output result reads "Sortie · Accessible" instead of
  "Sortie · Vérifiée": the check proves the output can be opened, not that
  the far end heard anything.
- The microphone check says "Parlez un peu plus fort" instead of "Micro
  prêt" when the measured level is below what a live session actually
  accepts, so setup can no longer end green on a level the session would
  treat as silence.
- The automatic voice match honours a voice's own recommendation of zero
  instead of replacing it with the general default.
- A microphone or output you chose explicitly stays chosen: if it
  disappears, the app reports it as missing instead of silently starting on
  the system default. Defaults are still suggested for a route you never
  picked.
- The voice-engine download narrates its real states ("Préparation du
  téléchargement…", "Vérification du téléchargement en cours…",
  "Interruption en cours…") instead of a bare percentage.
- Listening to yourself narrates its own states and refuses politely
  instead of failing when it cannot run ("Arrêtez le direct avant de vous
  écouter.", "Pour vous écouter, configurez d'abord le micro et la
  sortie.").
- The voice-name dialog's language option reads "Anglais" instead of
  "English" inside the French interface, and several voice and face pages
  carry shorter, plainer copy.
- The failed-stop message offers the useful next step first: "L'arrêt de
  la voix a échoué. Réessayez. Si cela échoue encore, fermez puis rouvrez
  LiveCam."
- Settings diagnostics report one update status instead of two fields that
  could disagree, and the copied support report carries that same value.

### Fixed

- Live voice no longer drops out when you start speaking again after a
  pause: at the default responsiveness setting the graphics device was
  allowed to fall asleep during silence, and the first phrase back paid a
  cold start the audio buffer could not cover. A 20 minute reference run
  went from 33 drop-outs to 2.
- Live voice on a microphone with a fixed electrical offset: such an input
  used to hold the speech detector open and push out-of-range audio into
  the conversion, roughly doubling the word error rate. The offset is now
  removed before anything else sees the audio.
- Live face never publishes a picture of an unconfirmed face: the output
  freezes from the very first missed check instead of continuing until an
  absence is confirmed, so the deformed frames at the moment someone leaves
  or returns are gone.
- A failed camera check is no longer read as "camera not installed": the
  check answers present, absent, or unknown, and only a real negative
  offers "Réparer la caméra". Opening a streaming application first no
  longer makes LiveCam recommend installing an already-installed camera.
- The virtual camera delivered roughly ten times its advertised frame rate
  to receiving applications; frames are now paced at the advertised rate.
- Listening to yourself and going live no longer fight over the same
  session: starting one while the other runs is refused cleanly, and a
  refusal can never stop a broadcast that is already running.
- A stop that fails or whose answer is lost keeps the session owned and
  visible until you stop it explicitly, on the dashboard and in the voice
  preview, instead of quietly showing "stopped" while audio may still flow.
- Saving a converted file can no longer erase an existing file before
  failing: the new file is written beside the target and put in place in
  one step, so a locked destination fails visibly and the original
  survives.
- Several downloads running at once no longer refuse a healthy slow
  connection: they share one connection budget instead of each demanding
  the whole floor, a cancellation is observed within a fraction of a second
  instead of up to a minute later, and a slow but durable save is no longer
  misread as a stalled network.
- Retrying a microphone or speaker after a failure targets the same
  physical device even when Windows has renumbered it, and a level meter
  that was still closing, or a device rescan already in flight, no longer
  refuses the whole start.
- Recovering from a device change mid-call names the interruption instead
  of claiming the voice could not start, and a recovery whose internal
  reset fails is treated as incomplete instead of reported as recovered.
- The live session no longer fails to start when its automatic
  responsiveness setup gets one refused answer from the service after it
  had already answered once, and a cached permission from an earlier
  measurement can no longer grant the faster setting to a machine the
  current measurement refuses.
- "Licence vérifiée" is shown only after a real online verification of an
  active licence; cached, offline and inactive results get their own
  accurate wording, and two settings actions checking the licence at the
  same time no longer make each other fail.
- The updates card no longer contradicts itself: a failed check states its
  own next step and outranks a stale "up to date", while a known available
  update stays offered.
- A missing or mismatched video component says to reinstall instead of
  suggesting a retry that cannot work, and a genuine timeout still says
  timeout.
- Auditioning speakers in a multi-speaker recording plays the right person,
  and loading one preview no longer moves the next choice under the
  pointer.
- A notification can no longer sit on top of the Stop button on the
  dashboard or the voice page, and clicking Stop no longer just dismisses
  the notice. Persistent notices gained an explicit close that does not
  stop audio.
- The face page's empty panel follows the light appearance instead of
  staying dark, and the consent dialog's long paragraphs stay inside the
  card at the default and minimum window sizes.
- Changing voices or settings during a preview can no longer arrive out of
  order: an explicit stop wins over a start still in flight, and a late
  success can no longer repaint "listening" after you stopped.
- Choosing "Continuer sans vérifier" for the microphone is accepted
  everywhere, so the preview, the start button and the guidance no longer
  loop back to setup.
- Deleting a face that may still be on camera is refused with honest
  wording, and an unrelated library face stays deletable.
- An oversized or unreadable style photo is refused with guidance ("Cette
  photo est trop lourde. Choisissez une photo plus légère.") instead of
  being accepted and then rejected later.
- Renaming, deleting and cancelling voices report their real outcome,
  including when a cancellation could not be confirmed or a voice is still
  in use by another application.
- Aborting the voice-engine installation during its reference download now
  actually stops the installation, and cancelling the engine download is
  confirmed before the dialog claims it stopped.
- Leaving a screen no longer abandons work that must finish: a microphone
  preview and a library import stay owned until the backend acknowledges,
  while a long voice creation still lets you move between tabs.
- A refused choice on go-live (wrong output, missing device) is answered
  with its own French explanation instead of being handled as an internal
  fault.
- The automatic responsiveness verdict served from the cache now carries
  its remaining lifetime instead of a fresh one, so a session cannot keep a
  faster setting past the measurement that granted it.

### Release pipeline

- Every desktop publish since 0.6.0 reported a failure after a complete
  publish: the updater feed was written with Windows line endings on the
  build runner, committed to the updates repository as Unix line endings,
  and then compared byte for byte against the served copy. The feed is now
  written with Unix line endings everywhere.
- The hosted first-run smoke read the stored activation from a field the
  desktop stopped writing on 2026-08-20, so every hosted run since 0.5.0
  failed after a complete, successful first run and leaked its smoke seat.
  It now opens the protected record the way the desktop does, and keeps
  the app's logs for the post-mortem step that used to find nothing.
- The daily channel-lifetime check ran without the credentials it needs,
  so it could never reach a verdict and failed every day.

---

The installer above is the only file to download; the source archives are GitHub's automatic copy of this note.
