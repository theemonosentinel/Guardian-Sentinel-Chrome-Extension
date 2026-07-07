Guardian v2.0 — Contextual AI Companion (Chrome Extension)
A quiet, gesture-summoned AI sidekick that runs entirely on-device using Chrome's built-in Gemini Nano model. No API key, no login, no subscription, nothing ever leaves your machine (except the optional DuckDuckGo lookup if you flip on "Internet Access").

1. Install the extension (unpacked)
Open chrome://extensions
Turn on Developer mode (top right)
Click Load unpacked
Select this guardian-extension folder
2. Enable the on-device AI (Gemini Nano)
Chrome's on-device model is behind flags until it's fully rolled out. One-time setup:

Go to chrome://flags/#optimization-guide-on-device-model → set to Enabled BypassPerfRequirement
Go to chrome://flags/#prompt-api-for-gemini-nano → set to Enabled
Relaunch Chrome
Go to chrome://components, find Optimization Guide On Device Model, click Check for update — it needs to download (~1-2GB), give it a few minutes
Click the Guardian icon in your toolbar — it'll tell you if the model is ready
You only have to do this once. After that, Guardian works completely offline.

3. How to summon Guardian
Dual-Orbit (primary): hold Left Alt and draw two full circles with your mouse — no clicking or dragging needed, just move the cursor while Left Alt is down. A faint blue tracer follows your cursor and pulses once when it registers. Do the gesture again (Left Alt + two circles) while the panel's open to close it. Releasing Left Alt mid-circle cancels the attempt, so half-finished circles never accidentally fire.

Peripheral Rapid-Click (secondary): 5 left-clicks within 2 seconds, right at the edge of the page (not on the scrollbar itself — the strip just to its left). Dots fill up on the edge as you go. If your browser's scrollbar eats the clicks, drag the window slightly narrower first, or just use the right-click fallback below.

Alt + Highlight (quick actions): hold Alt and select some text — a small pill pops up with Summarize / Explain Code / Translate, no full panel needed.

Right-click fallback (always works): highlight any text, right-click it, and choose "Open Guardian Panel" from the context menu. This doesn't depend on any gesture detection at all — good for when you just want it open, no fuss.

4. Settings (toolbar popup)
Guardian Enabled — master on/off switch
Internet Access — when on, Guardian does a quick DuckDuckGo lookup (their free, keyless Instant Answer API) to add live context before answering. Off by default — fully offline otherwise.
5. Notes on this build
OCR (for canvas/image text) uses Tesseract.js, bundled locally in lib/tesseract/ — no CDN calls, so it stays Manifest V3 / Web Store compliant. First OCR call may take a second to spin up the worker.
Context persists while you're on a page; hit Clear in the panel header to manually wipe it. Nothing is auto-deleted out from under you.
UI renders inside a closed Shadow DOM purely so host page CSS can't leak in or clash with Guardian's styles — it's not trying to evade any security or detection tooling.
Alt-state is deliberately NOT reset on window "blur" — Chrome can fire a transient blur just from pressing Alt alone (it flicks focus toward the browser's own UI), which was silently cancelling gestures before. State resets on focus-regain instead, which is safer.
The passive cursor-cadence dashboard from the original spec was deferred — this build focuses on gesture detection + the AI panel.
File structure
guardian-extension/
├── manifest.json
├── background/background.js      # settings, context menu, web-search fetch (privileged, cross-origin)
├── content/content.js            # gestures, shadow DOM panel, AI calls, OCR — the real engine
├── popup/                        # toolbar popup (toggles + AI status check)
├── lib/tesseract/                # bundled OCR engine (no remote code)
└── icons/
