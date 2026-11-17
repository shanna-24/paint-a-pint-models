# Paint-A-Pint models

Models that the Paint-A-Pint image editor for the Mac downloads once, when the person asks. They then run on the
Mac. Pictures never leave the Mac.

The Real-ESRGAN model for Upscale and Enhance is included with Paint-A-Pint and is no longer hosted here.

The app pins each archive's size and SHA-256 and refuses anything else. Archives hold a Core ML package (`.mlpackage`) or an atomic bundle of packages and prompt assets. Each includes
its `LICENSE` and `NOTICE`; the app compiles the packages on the Mac when it installs them. `SHA256SUMS` lists the current archives.

## LaMa 1024 for Remove Object (release `lama-v4`)

`LaMa-1024.mlpackage.zip`: a Core ML ML program (fp32 weights and maths, macOS 13 or later) of
[LaMa](https://github.com/advimman/lama) (big-lama), "Resolution-robust Large Mask Inpainting with Fourier
Convolutions", Suvorov et al., Samsung AI Center, 2021, at 1024 × 1024.

- Size: 190,037,386 bytes
- SHA-256: `6565a9f6c271cbaa437b49f683898b00a0818575dfcc7933e12524babf3b0294`
- Input `image`: 1×3×1024×1024 float RGB, 0–1. Input `mask`: 1×1×1024×1024, 1 where to fill.
- Output `output`: 1×3×1024×1024 float RGB, 0–1.
- About 1.2 s a pass on an M4's GPU.

It was converted from big-lama's TorchScript weights (`big-lama.pt` from the IOPaint project's
[model releases](https://github.com/Sanster/models/releases/tag/add_big_lama), SHA-256
`344c77bbcb158f17dd143070d1e789f38a66c04202311ae3a258ef66667a9ea9`) by `scripts/remove-object-model/convert.py` in
the Paint-A-Pint source. The script traces the model at 1024 × 1024, converts it to a Core ML ML program (Core ML
takes the Fourier units' `rfftn` / `irfftn` as they are) and checks it against PyTorch (within 1 level of 255 on the
test photos). Note: LaMa was trained on Places365, whose images are provided for non-commercial research and
education.

## DDColor Tiny for Colourise (release `ddcolor-tiny-v1`)

`DDColor_Tiny.mlpackage.zip`: the DDColor Tiny Core ML package for macOS 13 or later, originally published by
CoreML-Models and hosted by mlboydaisuke/coreml-zoo. Paint-A-Pint validates it against the official DDColor Tiny
checkpoint.

- Size: 212,349,536 bytes
- SHA-256: `5d7aab178f0d6e20ea0148a8e733b3b71cef4e51794c37b1f14210903aeea927`
- Input `image`: 1×3×512×512 float RGB, 0–1; the model owns normalisation.
- Output `ab_channels`: 1×2×512×512 float CIE Lab chroma.

The archive adds `LICENSE` and `NOTICE` to the original, unchanged model package, whose archive has SHA-256
`bfecea37d66005f602efe13978360b8e4707923234c3d1d00beeb4e36cb1b02c`. See the
[DDColor credits](licenses/ddcolor/NOTICE) for the original source, checkpoint, converter and archive revisions.

## MobileSAM for Object Selection (release `mobilesam-v1`)

[`MobileSAM.bundle.zip`](https://github.com/shanna-24/paint-a-pint-models/releases/download/mobilesam-v1/MobileSAM.bundle.zip)
contains the MobileSAM TinyViT encoder, a fixed two-token mask decoder, learned prompt weights, an integrity
manifest, `LICENSE` and `NOTICE` under one `MobileSAM.bundle` root. Both packages install atomically.

- Size: 20,178,610 bytes (about 20.2 MB)
- SHA-256: `73d41bb47c1a652f19871b82b7c0a2b03206b820371185a9d693f25dad9a8c09`
- Supported prompts: two box corners, or one positive click plus padding. Arbitrary multi-point refinement is not supported.
- Whole-image longest-side-1024 inference; boxes guide selection but do not crop inference to add detail.
- Validated on macOS 26.6.2 arm64 with CPU_AND_GPU. The app currently enables Apple silicon on macOS 26.6.2 or later.
  macOS 12 is the export deployment target, not a claim of tested older-system or Intel compatibility.
- Ten representative box/click prompts matched the original PyTorch model at chosen-mask IoU 0.99408–1.0, with
  identical selected candidate indices, repeatable results and no E5RT diagnostics. This measures conversion parity,
  not accuracy against human-labelled outlines. [Validation data](validation/mobilesam-v1/validation.json).

The encoder and learned prompt-weight JSON originate from john-rocky/SamKit v1.0.0; Paint-A-Pint re-exports the
original learned decoder weights to a genuinely fixed two-token graph. See [credits and provenance](licenses/mobilesam/NOTICE).
The preparation and parity-verification recipe is in `scripts/object-selection-model` on Paint-A-Pint's
`codex/object-selection` feature branch. The full manual native UI pass remains outstanding; this model release
is not an announcement that the application feature has shipped. Images remain on the Mac.

## Licences and credits

Each model is redistributed under its own licence, and copyright remains with its authors. These model licences do
not license Paint-A-Pint as a whole.

- LaMa (Apache License 2.0): [licence](licenses/lama/LICENSE), [credits and provenance](licenses/lama/NOTICE).
- DDColor Tiny (Apache License 2.0): [licence](licenses/ddcolor/LICENSE), [credits and provenance](licenses/ddcolor/NOTICE).

- MobileSAM (Apache License 2.0): [licence](licenses/mobilesam/LICENSE), [credits and provenance](licenses/mobilesam/NOTICE).

Paint-A-Pint also bundles these texts in Help → Third-party licences for offline access.
