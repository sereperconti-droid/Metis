# METIS — QA interno

## Stato corrente
- JavaScript syntax: PASS
- Service worker syntax: PASS
- HTML parsing: PASS
- HTML id uniqueness: PASS (duplicate planContext corrected)
- External fetch allowlist: PASS
- No analytics/ad SDK detected: PASS
- Permissions-Policy denies camera/microphone/geolocation: PASS
- PWA shell/cache structure: PASS
- Brand asset remains the approved METIS reference: PASS

## Spesa
- Product discovery uses public Open Food Facts data: PASS
- Price lookup uses public Open Prices endpoints when available: PASS
- Habitual brands are tried before generic product fallback: PASS
- City matching is used only as a ranking signal when present in public price data: PASS
- Missing price is never replaced with an invented value: PASS
- Total source outage is distinguished from “no matching public product”: PASS
- Search results can be carried into Piani intelligenti: PASS

## Carburante
- City/province public search: PASS
- Fuel type filtering: PASS
- Price sorting: PASS
- Fill-cost calculation: PASS
- Public-source/date information is surfaced when available: PASS
- Missing source data is not replaced with example prices: PASS
- Fuel results can be carried into Piani intelligenti: PASS
- MIMIT is treated as the originating institutional source; current public data are daily open data. citeturn0search0turn0search5

## Piani intelligenti
- A fuel search can be transferred into the plan: PASS
- A grocery search can be transferred into the plan: PASS
- Best observed fuel price can be inserted into the net-saving calculation: PASS
- Grocery total is explicitly treated as an observed-price subtotal, not a guaranteed basket price: PASS
- Net fuel advantage subtracts the estimated fuel cost of an extra detour when vehicle consumption is available: PASS
- No automatic route distance is fabricated: PASS
- Google Maps is used only as an external route handoff: PASS

## Energy constraint retained
METIS does not invent the annual cost of variable energy offers. ARERA's methodology uses forward values updated monthly for variable-price offer estimates; a simple current spot value would not be equivalent. citeturn0search3turn0search22

## Remaining device-level validation
- Safari/iOS real-device behavior for file picker, PDF.js, OCR, WebGPU/WebLLM, memory pressure and PWA suspension/resume.
- Real-network CORS/availability of public sources (requires real Safari/iPhone session).
- Route distance calculation remains intentionally external/manual until a verified routing source is selected.

## Safari/iPhone hardening — 2026-10-24
- Confirmed METIS currently does not use IndexedDB or FormData for document handling.
- PDF path reads the selected file into an ArrayBuffer and parses it locally with PDF.js.
- OCR path now converts the user-selected image File into a fresh Blob from ArrayBuffer before passing it to Tesseract. This avoids relying on a disk-backed File object in downstream processing.
- No document/file upload endpoint is implemented.
- Manifest uses `display: standalone` for Home Screen web app behavior.
- Service worker caches only the app shell and same-origin GET assets; external data/model requests are not cached by the service worker.
- `node --check app.js` passed after the hardening change.
- HTML parser check passed.
- External network calls remain intentionally unclaimed as end-to-end verified until tested on a real iPhone/Safari session.

## Final-chain hardening — 2026-10-24
- Grocery source failures are no longer presented as “no product found” when all public product searches fail; the UI now distinguishes source outage from a genuine empty result. PASS
- WebLLM model ID used by the build was rechecked against the current WebLLM prebuilt catalog: `SmolLM2-360M-Instruct-q4f32_1-MLC` is listed and requires about 579.61 MB VRAM; it does not require `shader-f16`. PASS citeturn1search1turn1search5
- WebLLM's current default cache backend is Cache API, not IndexedDB, reducing exposure to the Safari IndexedDB/File issue in the optional local-AI path. PASS citeturn1search1
- Safari 26 supports WebGPU and Home Screen web apps; feature detection remains the gate for optional local AI. PASS citeturn0search3
- Tesseract.js browser worker configuration was checked against its documented `workerPath`/`corePath`/`langPath` model; the configured core path is a directory, allowing device-appropriate core selection. PASS citeturn1search2turn1search7
- Real-device Safari/network validation remains the only unverified layer.
