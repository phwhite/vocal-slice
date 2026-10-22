# Changelog

**This file is the source of truth for every release's notes, past and present.** Never type notes into
the GitHub UI — edit them here and push, so the two can't drift apart.

`build-scripts/release-notes.js` lifts the **`## Unreleased`** section into the draft's "What's new"
during `npm run release:win` / `release:mac`. Write bullets here as you make the change, rather than
reconstructing them at release time.

| Task | Command |
| --- | --- |
| Close out a shipped version | `npm run changelog:promote` |
| Update the public changelog page | `npm run changelog:site` |
| Fix the release currently in flight | `npm run release-notes -- --refresh` |
| Fix an **already-published** release | `npm run release-notes -- --notes=1.421.0` |

`--notes` rewrites that release's "What's new" and **nothing else** — it never touches the SHA-256 table
and never reads `dist/`. That separation is deliberate: the table is derived from whatever is in `dist/`
at the time, so a shared code path would republish an old release's checksums as hashes of the *current*
build — wrong values for binaries people have already downloaded.

Promotion happens **after** publishing, not before: the version is `2.{git rev-list --count HEAD}.0`, so
the commit that renamed a heading would itself bump the count and make that heading wrong by one.

## Unreleased

- Fixed transcription getting stuck forever when Whisper falls into a loop on a stretch it can't make out — music, silence, background noise — repeating the same few words ("Cassie, the Cassie, the Cassie…") while the status sat on "Lining up the words". The transcription library had started re-reading such a stretch from the same point over and over; each stretch is now read once and the run moves on.
- When Whisper does repeat itself like that, the transcript no longer fills with the repeated phrase. You get the phrase once, then an **[unclear]** marker covering the rest of that stretch, so you can see where it happened and listen for yourself. The Log tab says what was collapsed and where.
- There's now a **Stop** button under the progress bar while a transcription is running on the graphics card, so a run that has gone wrong can be stopped without quitting the app. (On a processor-only run the window is frozen while it works, so the button isn't offered there.)
- You can now open MP4 and MOV videos. Vocal Slice uses the sound track and ignores the picture, so a phone recording, a screen capture or a video call export can be transcribed and sliced like any audio file. The file picker lists them alongside audio, and a video shows a film icon in the file bar.
- Press **M** while listening to drop a numbered marker at the playhead (or use the new bookmark button next to zoom). Markers show as blue lines on the waveform and blue tags in the transcript, and are listed in a pane beside the transcript where you can click one to jump there or double-click its name to rename it. Right-click any marker to rename it, delete it, or turn it straight into a selection that runs to the next marker. **[** and **]** jump between markers, and markers are saved with your session.
- The transcript now reads like prose. Each sentence starts on its own line, and a long pause (or a long run of sentences) starts a new paragraph, so you can find your place instead of facing one block of words. Older saved sessions get the same layout.
- A faint time stamp appears beside the first line of each paragraph, and small sentence numbers sit beside each sentence. They live in a margin, so selecting text never picks them up.
- When you zoom the waveform in, the words outside the zoomed range now fade, so you can see which part of the transcript you're looking at.
- You can now remove parts of a slice in the slice editor. Click a word on the waveform to cut it, press **X** once at the start and again at the end, or click the new scissors button and drag across the waveform. Cuts show as hatched regions: drag their edges to adjust them, and use the **×** or right-click to remove one. Right-click a cut and choose **Audition the join** to hear how the edit sounds. Play skips the cut parts, and the saved clip is the kept parts joined, with each edge snapped to a zero crossing so the joins don't click. The slice remembers its cuts, so you can reopen the editor later and change your mind. Edited slices show a scissors badge on their card.
- The slice editor has a volume slider, from silent up to 200%. Play follows it as you drag, and the saved clip is made louder or quieter to match, in the same format and quality as the original. Double-click the slider to put it back to 100%. If the setting would push the loudest parts past what the file can hold, the percentage turns red to warn you they'll clip. A slice with its volume changed shows the percentage on its card, and the editor remembers it when you reopen it.
- Slices cut from the demo clip are now saved as exact copies of the demo audio, the way slices from your own WAV files are. They used to be re-encoded.
- The transcript scrolls to centre the highlighted words whenever the selection changes for you: search next/previous, opening a slice, zooming. If the highlight is scrolled out of view, a small crosshair button appears at the top right of the transcript to jump back to it.
- The "open audio file" and "export slices" pickers now each remember their own folder. Choosing somewhere to export your clips used to move where the next file picker opened, so you'd land in your exports folder when you went to open the next recording.
- You can now delete several slices at once: tick them on the Slices tab and press **Delete selected**. One confirmation covers the lot, instead of a prompt per slice.
- The space bar now plays the selection on the Transcription tab, and pressing it again stops and jumps back to where it started, so a second tap replays the same passage. Click in the waveform (or jump to a marker) to move the playhead and space plays from there instead. The Play and Pause buttons still resume in place. Space used to re-press whichever button you'd clicked last, which was usually Create Slice, so tapping it made an unwanted clip.
- New zoom button next to the loop control. Highlight some words, narrow the range with the handles, then press it and the waveform reframes to just that range, the same way it does when you first highlight text. The slice editor has the same button.
- **Vocal Slice is now free and open source.** There's no trial and no licence key any more — every feature is unlocked for everyone, and the full source is on GitHub under the GPL-3.0. If you bought a licence, thank you: you don't need to do anything, and nothing you had stops working.
- The License page has gone from Settings, along with the trial countdown in the tab bar. Vocal Slice also tidies away the licence and trial records it used to keep on your computer.
- Settings → About now shows the open-source licence notice and a link to the source code.
- Version numbers now start at 2. That's the move to the open-source repository, not a change in how updates work.
- Fixed the release notes in Settings → About showing raw formatting marks — `**like this**` — instead of bold text and code.

## 1.475.0 — 2026-08-27

- The first-run tour now points out that selecting a phrase finds every other place it appears, so you can step between takes of the same line instead of hunting for them.

## 1.474.0 — 2026-08-26

- **Vocal Slice is now a one-time purchase.** Buy it once and it's yours — no subscription, no renewal, and every future version included. It's the same $29 it always was; you just don't pay it again.
- Once you've activated your key, Vocal Slice never needs to check in again. It used to re-confirm your subscription every couple of weeks, which meant a fortnight away from the internet could lock you out of your own software. That can't happen now: activate once and it keeps working, offline, indefinitely.
- Settings → License now tells you what it costs and what you get before you click through to the checkout, instead of sending you to a payment page to find out.
- The "Active until" line has gone from Settings → License. It never had a date to show, and a purchase that doesn't run out has no end date to report.

- Vocal Slice now tells you what it's doing in plain language while it gets started — "Downloading transcription model", "Reading your audio", "Transcribing" — instead of naming internal model files like `decoder_model_merged_q4.onnx`.
- The progress bar for a first-time model download now fills once, from start to finish. It used to restart from zero for each of the six files a model is made of, so it swept across six times and told you nothing about how much was actually left.
- If part of a model is already on your machine, the bar starts partway along rather than at zero, so it reflects what's genuinely left to download.
- Added **Whisper Medium**, in English-only and multilingual flavours — the most accurate model Vocal Slice offers, and by some way the slowest. It runs with or without a graphics card, though on a machine without one expect it to take several times longer than the audio itself; the smaller models remain much better company there.
- The model list is now grouped into English-only and multilingual, so it stays readable as it grows.
- Vocal Slice now asks before starting a model download over 1GB, and tells you how big it is, so a change of setting can't turn into a surprise 1.6GB download later on.
- Transcribing on your processor is now around two to three times faster. Vocal Slice was only ever using a single core for it, however many your machine has; it now uses half of them, leaving the rest free for whatever else you're doing. The transcript comes out identical — this is the same work spread wider, not a shortcut. Whisper Medium on a processor drops from roughly seven times the length of your audio to under three.
- Vocal Slice now warns you that it will stop responding while it transcribes on your processor, instead of simply freezing. Transcribing without a graphics card has always held the window still until the run finishes — on a long file with a large model that can be a good while, and with nothing on screen to say so it looked like a crash. Settings → Transcription says the same thing where you choose the processor.

- Transcription models now come from Vocal Slice itself rather than a third-party site. Previously the app downloaded them from Hugging Face, which meant that if those files ever moved or became unreachable, a fresh install had no way to transcribe anything. Nothing about your audio changes — it was always processed on your machine — but the app no longer depends on anyone else to work.
- Models are now saved as ordinary files in a folder you can open, back up, or copy to another machine. They used to live in hidden browser storage that nothing but the app could reach, and that "Clear Session Data" could wipe by accident.
- Every model file is checked against a known fingerprint as it downloads, so a partial or corrupted download is caught and fetched again instead of failing later with an unhelpful error.
- If our download servers can't be reached — some workplace networks block them — the app now quietly falls back to fetching the same model from Hugging Face, rather than leaving you unable to transcribe. The fingerprint check applies either way, so you always end up with the exact model we tested.
- The model picker now shows what each model will actually download on *your* machine. The old figures were badly wrong: "Whisper Small (~500MB)" is really about 563 MB on a graphics card, and closer to 928 MB without one. Sizes now reflect which of the two your machine will use.
- Settings → Storage now shows the real size of each downloaded model and where it lives on disk, and deleting one frees the space immediately.
- If you're upgrading, the old hidden model cache is cleared for you on first launch, which frees up the space it was holding.
- On macOS, fixed the download bar going missing when you closed the window and reopened Vocal Slice from the dock: the next model download reported nothing at all, so a fresh install looked frozen while it was quietly working.

- Fixed the **Third-party licenses** button in Settings → About, which did nothing at all in installed builds. The licences now open in a window inside the app, so you don't need anything installed to read them.

## 1.462.0 — 2026-08-18

- Settings → About now lists what changed in each release, newest first, so you can catch up on what you've been given without leaving the app. It's read from the copy shipped inside the app — nothing is fetched.
- When an update has installed, Vocal Slice now says so once and offers to show you what changed. Previously it updated silently and you had no way of knowing what was new.
- The first-run tour is worded more plainly.
- Your remaining trial days are now shown at the top of the window, next to the theme and GPU icons. Previously the countdown was only in Settings → License, so a trial could run out without you ever seeing a number. It turns amber for the last two days, and clicking it takes you to your licence options.
- When the trial ends, Vocal Slice now explains what's happened and offers to buy a licence or enter a key, instead of a bare message that dropped you into Settings.
- Creating a slice after the trial has ended now tells you why it wasn't saved. It used to fail quietly and the file was lost when you closed the app.

## 1.451.0 — 2026-08-11

- Vocal Slice now transcribes in all 99 languages Whisper knows, instead of the 14 that were listed. Greek, Turkish, Polish, Hebrew, Swahili and the rest are all there. Slicing works exactly as it does in English — select the words you want and cut. Pick one of the multilingual models for this; the English-only ones still only do English.
- The language picker is now a search box: start typing and it filters, so you don't scroll a list of 99. It finds a language by its English name, its own name, or its code — "greek", "Ελληνικά" and "el" all get you there, and accents are optional, so "espanol" finds Spanish.
- Your chosen language is no longer forgotten when you switch between two English-only models.
- You can now tick several slices in the Slices tab and export them all at once — pick a folder and they're saved into it with your filename template, instead of exporting them one at a time.
- You can drag a slice straight out of the app onto your desktop or into your DAW. Grab it anywhere on the slice except the playback controls or the waveform. If you've ticked several slices, dragging any one of them drags the whole set.

## 1.446.0 — 2026-08-01

- Transcription progress now shows on the app's taskbar button on Windows and its dock icon on macOS, so you can leave a long file running and check on it without switching back to the app. When it finishes the icon flashes if you're in another app.
- Settings → License now tells you what happens when the trial ends, instead of listing what the trial includes.
- The Subscribe button in Settings → License no longer has its text underlined.
- The file picker no longer offers WMA and AIFF. Neither could actually be opened, and choosing one gave you an error saying transcription had failed.
- A file that can't be opened now says so, and names the formats that work, instead of reporting that transcription failed.

## 1.437.0 — 2026-07-27

- First-run tutorial typo.

## 1.435.0 — 2026-07-26

- Fixed clips in the Slices tab being able to play over each other — editing a slice and
  previewing another could sound at the same time. Only one clip plays at a time now.
- Opening the file picker from the status bar no longer clears the demo (or your loaded file)
  when you cancel without choosing a new file.
- The first-run tutorial now also covers choosing a file, the model/language settings, and
  local GPU processing.

## 1.432.0 — 2026-07-26

First public release.

- Transcribes audio on your own machine — no uploads, no account, no telemetry.
- Cut clips by selecting words in the transcript; the waveform follows the selection.
- Lossless byte-level WAV slicing; other formats decode to 24-bit WAV.
- English and multilingual Whisper models, GPU accelerated via WebGPU with automatic CPU fallback.
- Filename templates, five themes, and a searchable transcript.
- Windows and macOS.
