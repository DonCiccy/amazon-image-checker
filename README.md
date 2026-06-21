# Amazon Image Checker

A fast, client-side tool that verifies product images against Amazon's 8 official main image requirements — **no AI, no API, no backend, no cost**.

**[→ Live demo](https://DonCiccy.github.io/amazon-image-checker)**

---

## What it checks

| # | Requirement | Method |
|---|-------------|--------|
| 1 | Pure white background | Automated — corner pixel sampling |
| 2 | Frame fill ≥85% | Automated — product bounding box analysis |
| 3 | No decorative borders or frames | Automated — edge uniformity detection |
| 4 | Product fully visible, not cropped | Automated — edge pixel detection |
| 5 | Image quality and resolution (≥1000px) | Automated — Laplacian sharpness + dimensions |
| 6 | No text or watermarks | Manual visual confirmation |
| 7 | No non-included accessories or props | Manual visual confirmation |
| 8 | Photographic image, not illustration | Manual visual confirmation |

The 5 automated checks run instantly via Canvas pixel analysis the moment an image is uploaded. The 3 checks that require semantic understanding are presented as interactive checkboxes for the user to confirm visually. Each failed check includes a specific, actionable "how to fix" tip.

---

## How to use

1. Open `index.html` in any modern browser — or visit the [live demo](https://DonCiccy.github.io/amazon-image-checker)
2. Upload your product image (JPG, PNG, WEBP — up to 10 MB)
3. Read the 5 automated results
4. Review the preview and tick the 3 visual confirmation checkboxes
5. Fix any issues using your preferred image editor
6. Copy the formatted result summary to share or document

No internet connection required after the page has loaded.

---

## How the pixel analysis works

All analysis runs on a 640px downscaled canvas for performance. Original dimensions are preserved and used only for the resolution check.

**Background (check 1)**
Samples the four 10% corner regions of the image. If ≥97% of sampled pixels have all RGB channels ≥ 235, the background passes as near-white.

**Frame fill (check 2)**
Finds the bounding box of all non-white pixels and calculates what percentage of the total image area it covers. Requires ≥85% to pass.

**No borders (check 3)**
Examines the outermost ~1% pixel ring. If a significant portion is uniformly non-white (low colour variance across the ring), a decorative border is flagged.

**Fully visible (check 4)**
If any non-white pixel appears within 2px of any image edge, the product is likely clipped and the check fails.

**Image quality (check 5)**
Checks original pixel dimensions (warns below 1000px, fails below 500px), then computes the mean absolute Laplacian on non-white pixels only to detect blur without being misled by the white background.

---

## Privacy

Everything runs entirely in the browser. No image data is uploaded, transmitted, or stored anywhere. The tool has no backend, no analytics, no cookies, and no external dependencies.

---

## Tech stack

- Single HTML file — vanilla JS, CSS, no frameworks, no dependencies
- Canvas API for pixel-level image analysis
- FileReader API for local file loading
- `navigator.clipboard` for result copying

---

## Limitations

Three of the eight checks (text/watermarks, props, photographic) require semantic image understanding and cannot be reliably automated without a trained vision model. The tool handles these as guided manual checkboxes, making the limitation explicit rather than hiding it.

---

## License

MIT — free to use, modify, and redistribute.
