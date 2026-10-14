# I-Log LUT Tuner

A single-page, in-browser tool for making `.cube` and `.3dl` LUTs from Insta360 I-Log footage. It is inspired by GoPro Labs' GP-Log2 LUT Generator. Everything runs locally in your browser: your LUTs, images and videos are never uploaded.

> Not affiliated with Insta360 or GoPro.

## What it does

**Two modes**

- **Official LUT + adjustments:** load Insta360's official I-Log `.cube` and add exposure, white balance, rolloff, contrast, saturation, lift/gamma/gain and more on top.
- **Direct conversion:** builds a LUT from the I-Log curve alone (no official LUT needed) and outputs Rec.709, Rec.2020 SDR, Rec.2020 HLG or Rec.2020 PQ.

**Other features**

- EV, temperature and tint applied in scene-linear light *before* the base LUT, like GoPro's generator.
- **Match official LUT:** fits the direct-mode controls to Insta360's official look and reports how close it got.
- Adjustment strength slider, presets (saved in your browser), and adjustable highlight shoulder and gamma.
- Live preview on a still image, a video frame or a built-in test chart, with a before/after split, zebra clipping overlay, zoom, RGB parade, luma waveform and vectorscope.
- Export as `.cube` or `.3dl`, one size or 17/33/65 together, in a `.zip` with real file extensions.

## How to use

1. Open the page (or `index.html` locally).
2. *(Optional)* Download the official I-Log LUT for your camera from [Insta360's I-Log downloads page](https://www.insta360.com/download/i-log) and load it.
3. Pick a mode, set your adjustments and watch the preview.
4. Press **Save as .zip** and load the `.cube` in your editor, with your footage set to I-Log.

## How it works

The I-Log transfer function and BT.2020 gamut come from Insta360's published *10-bit I-Log White Paper*. The code reproduces the paper's table values (black at code 93, 18% gray at 432, 2200% at 1023 on a 10-bit scale). Interpolation of loaded LUTs uses tetrahedral interpolation.

## Known limitations

- The white paper is for Insta360's 10-bit I-Log. It has **not** been confirmed that every camera, including the X5, uses exactly this curve. The *Match official LUT* check can detect gamut mismatches but cannot prove the curve is correct, because the tone controls can absorb small differences.
- Direct mode uses a simple tone mapping. It will not look identical to Insta360's official LUT unless you use *Match official LUT*.
- `.3dl` files are written as 10-bit in and out. Dialects vary between programs, so prefer `.cube`.
- HDR (HLG/PQ) output is untested on a real HDR display.
- Video preview depends on browser codecs. ProRes only plays in Safari, and HEVC needs system support. Use a still frame instead. Browser-decoded colors are approximate.
- Not yet validated against real X5 footage.

## Deploy on GitHub Pages

Put `index.html` in the root of a public repo, then go to **Settings → Pages**, choose **Deploy from a branch** (`main`, `/ (root)`) and save. No build step is needed.

## How AI was used

This tool was written with **Claude**, an AI assistant made by Anthropic, in a single conversation. The author described what they wanted and directed the work. Claude researched the I-Log specification, wrote the code, and tested the maths with scripts. This included checking the curve against the white paper's table, confirming that an identity LUT round-trips exactly, checking `.3dl` ordering and validating the `.zip` output.

Please treat the code accordingly:

- The colour maths has been tested with scripts, **not** against real footage, an HDR display or a range of browsers.
- Slider ranges, preset values and scope behaviour are design choices that have not been tuned by experts.
- Review the output on your own footage before relying on it for client work, and report any problems.

## Credits

- Insta360 *10-bit I-Log White Paper* for the curve and gamut, and the [official I-Log LUTs](https://www.insta360.com/download/i-log).
- GoPro Labs' GP-Log2 LUT Generator for the idea.
- Standard formulas: ITU-R BT.709 and BT.2020, HLG (BT.2100) and PQ (SMPTE ST 2084).

## License

MIT. See [LICENSE](LICENSE).
