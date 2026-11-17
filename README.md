# Paint-A-Pint models

Models that the Paint-A-Pint image editor for the Mac downloads once, when the person asks. They then run on the
Mac. Pictures never leave the Mac.

The Real-ESRGAN model for Upscale and Enhance is included with Paint-A-Pint and is no longer hosted here.

The app pins each archive's size and SHA-256 and refuses anything else. Every archive holds a Core ML package
(`.mlpackage`) with its `LICENSE` and `NOTICE` at the archive's root; the app compiles the package on the Mac when it
installs it. `SHA256SUMS` lists the current archives.

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

## Licences and credits

Each model is redistributed under its own licence, and copyright remains with its authors. These model licences do
not license Paint-A-Pint as a whole.

- LaMa (Apache License 2.0): [licence](licenses/lama/LICENSE), [credits and provenance](licenses/lama/NOTICE).
- DDColor Tiny (Apache License 2.0): [licence](licenses/ddcolor/LICENSE), [credits and provenance](licenses/ddcolor/NOTICE).

Paint-A-Pint also bundles these texts in Help → Third-party licences for offline access.
