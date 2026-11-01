# Paint-A-Pint models

Models that the Paint-A-Pint image editor for the Mac downloads once, when the person asks. They then run on the
Mac. Pictures never leave the Mac.

## LaMa 1024 for Remove Object (release `lama-v3`, current)

`LaMa-1024.mlmodelc.zip`: a compiled Core ML model (fp32 weights and maths, macOS 13 or later) of
[LaMa](https://github.com/advimman/lama) (big-lama), "Resolution-robust Large Mask Inpainting with Fourier
Convolutions", Suvorov et al., Samsung AI Center, 2021, at 1024 × 1024.

- Size: 190,058,703 bytes
- SHA-256: `39e98eb2999bd8b055a1c546d2b6842e73bb57fd41c62f3621e1a3ebb2b5dce5`
- Input `image`: 1×3×1024×1024 float RGB, 0–1. Input `mask`: 1×1×1024×1024, 1 where to fill.
- Output `output`: 1×3×1024×1024 float RGB, 0–1.
- About 1.1 s a pass on an M4's GPU.

It was converted from big-lama's TorchScript weights (`big-lama.pt` from the IOPaint project's
[model releases](https://github.com/Sanster/models/releases/tag/add_big_lama), SHA-256
`344c77bbcb158f17dd143070d1e789f38a66c04202311ae3a258ef66667a9ea9`) by `scripts/remove-object-model/convert.py` in
the Paint-A-Pint source. The script traces the model at 1024 × 1024, converts it to a Core ML ML program (Core ML
takes the Fourier units' `rfftn` / `irfftn` as they are), checks it against PyTorch (within 1 level of 255 on the
test photos), and compiles it. Paint-A-Pint deletes a `lama-v1` model once this one is installed.

The `lama-v3` archive adds `LICENSE` and `NOTICE` alongside the compiled model. Its model files are byte-for-byte identical to `lama-v2`; only the archive checksum changes. The original `lama-v2` release remains available for older apps.

## DDColor Tiny for Colourise (release `ddcolor-tiny-v1`)

`DDColor_Tiny.mlpackage.zip` is the DDColor Tiny Core ML package for macOS 13 or later, originally published by CoreML-Models and hosted by mlboydaisuke/coreml-zoo. Paint-A-Pint validates it against the official DDColor Tiny checkpoint.

- Size: 212,349,536 bytes
- SHA-256: `5d7aab178f0d6e20ea0148a8e733b3b71cef4e51794c37b1f14210903aeea927`
- Input `image`: 1 x 3 x 512 x 512 float RGB, 0-1; the model owns normalization.
- Output `ab_channels`: 1 x 2 x 512 x 512 float CIE Lab chroma.
- The app compiles the package locally, preserving the source image's size, brightness detail and transparency.

The archive adds `LICENSE` and `NOTICE` alongside the original, unchanged model package. The original archive has SHA-256 `bfecea37d66005f602efe13978360b8e4707923234c3d1d00beeb4e36cb1b02c`. See [DDColor credits](licenses/ddcolor/NOTICE) for the original source, checkpoint, converter and archive revisions.

## LaMa for Remove Object (release `lama-v1`, superseded)

`LaMa.mlmodelc.zip`: the same model at 512 × 512, used by Paint-A-Pint until 20 October 2026. Kept for copies of the
app built before then.

- Size: 190,021,167 bytes
- SHA-256: `aadf5d470c09ab0881f00084f27775574739b0e4d8860c14c10e3f64ec953d79`
- Input `image`: 1×3×512×512 float RGB, 0–1. Input `mask`: 1×1×512×512, 1 where to fill.
- Output `output`: 1×3×512×512 float RGB, 0–255.

It was converted from Carve's ONNX export ([Carve/LaMa-ONNX](https://huggingface.co/Carve/LaMa-ONNX),
`lama_fp32.onnx`, SHA-256 `1faef5301d78db7dda502fe59966957ec4b79dd64e16f03ed96913c7a4eb68d6`), whose Fourier
transforms are fixed 512 px matrices: the script then pinned the batch size, simplified the graph, rewrote the FFT
units' reversing slices as gathers, converted through PyTorch to a Core ML ML program, checked it against
onnxruntime (within 1 level of 255 on the test photos), and compiled it.

## Licences and credits

The models are redistributed under Apache License 2.0. Copyright remains with their respective authors. These model licences do not license Paint-A-Pint as a whole.

- LaMa: [full original licence](licenses/lama/LICENSE), [credits and provenance](licenses/lama/NOTICE).
- DDColor Tiny: [full original licence](licenses/ddcolor/LICENSE), [credits and provenance](licenses/ddcolor/NOTICE).

Current release archives include these texts as `LICENSE` and `NOTICE` at the archive root. Paint-A-Pint also bundles them in Help → Third-party licences for offline access. Existing releases are retained unchanged for older apps.

Note: LaMa was trained on Places365, whose images are provided for non-commercial research and education.
