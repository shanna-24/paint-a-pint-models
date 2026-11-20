# Slexip models

Models that the Slexip image editor for the Mac downloads once, when the person asks. They then run on the
Mac. Pictures never leave the Mac.

The Real-ESRGAN model for Upscale and Enhance is included with Slexip and is no longer hosted here.

The app pins each archive's size and SHA-256 and refuses anything else. Every archive holds a Core ML package
(`.mlpackage`) with its `LICENSE` and `NOTICE` at the archive's root; the app compiles the package on the Mac when it
installs it. `SHA256SUMS` lists the current archives.

## LaMa 1024 for Remove Object (release `lama-v7`)

`LaMa-1024.mlpackage.zip`: a Core ML ML program (fp32 weights and maths, macOS 13 or later) of
[LaMa](https://github.com/advimman/lama) (big-lama), "Resolution-robust Large Mask Inpainting with Fourier
Convolutions", Suvorov et al., Samsung AI Center, 2021, at 1024 × 1024.

- Size: 190,037,260 bytes
- SHA-256: `a013498bd81ce0e0d98fc099fc4b44f094369b782350ec8effef7776a6a83ad0`
- Input `image`: 1×3×1024×1024 float RGB, 0–1. Input `mask`: 1×1×1024×1024, 1 where to fill.
- Output `output`: 1×3×1024×1024 float RGB, 0–1.
- About 1.2 s a pass on an M4's GPU.

It was converted from big-lama's TorchScript weights (`big-lama.pt` from the IOPaint project's
[model releases](https://github.com/Sanster/models/releases/tag/add_big_lama), SHA-256
`344c77bbcb158f17dd143070d1e789f38a66c04202311ae3a258ef66667a9ea9`) by `scripts/remove-object-model/convert.py` in
the Slexip source. The script traces the model at 1024 × 1024, converts it to a Core ML ML program (Core ML
takes the Fourier units' `rfftn` / `irfftn` as they are) and checks it against PyTorch (within 1 level of 255 on the
test photos). Note: LaMa was trained on Places365, whose images are provided for non-commercial research and
education.

## DDColor Tiny for Colourise (release `ddcolor-tiny-v4`)

`DDColor_Tiny.mlpackage.zip`: the DDColor Tiny Core ML package for macOS 13 or later, originally published by
CoreML-Models and hosted by mlboydaisuke/coreml-zoo. Slexip validates it against the official DDColor Tiny
checkpoint.

- Size: 212,427,667 bytes
- SHA-256: `7737d722340d1fbec11e87d24f9c4c3dc4d5eec5a2e652d7d0c9b55cef0fa67a`
- Input `image`: 1×3×512×512 float RGB, 0–1; the model owns normalisation.
- Output `ab_channels`: 1×2×512×512 float CIE Lab chroma.

The archive adds `LICENSE` and `NOTICE` to the original, unchanged model package, whose archive has SHA-256
`bfecea37d66005f602efe13978360b8e4707923234c3d1d00beeb4e36cb1b02c`. See the
[DDColor credits](licenses/ddcolor/NOTICE) for the original source, checkpoint, converter and archive revisions.

## Rename release

The current releases refresh the Slexip notices and repository links. The model packages and upstream licence texts are byte-for-byte unchanged from the previous releases; existing installed models do not need to be downloaded again. Previous release assets are retired only after verified offline backups and successful app/client migration; older app versions may then require the offline recovery archives.

## Licences and credits

Each model is redistributed under its own licence, and copyright remains with its authors. These model licences do
not license Slexip as a whole.

- LaMa (Apache License 2.0): [licence](licenses/lama/LICENSE), [credits and provenance](licenses/lama/NOTICE).
- DDColor Tiny (Apache License 2.0): [licence](licenses/ddcolor/LICENSE), [credits and provenance](licenses/ddcolor/NOTICE).

Slexip also bundles these texts in Help → Third-party licences for offline access.
