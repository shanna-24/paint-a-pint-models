# Paint-A-Pint models

Models that the Paint-A-Pint image editor for the Mac downloads once, when the person asks. They then run on the
Mac. Pictures never leave the Mac.

## LaMa for Remove Object (release `lama-v1`)

`LaMa.mlmodelc.zip`: a compiled Core ML model (fp32 weights and maths, macOS 13 or later) of
[LaMa](https://github.com/advimman/lama) (big-lama), "Resolution-robust Large Mask Inpainting with Fourier
Convolutions", Suvorov et al., Samsung AI Center, 2021.

- Size: 190,021,167 bytes
- SHA-256: `aadf5d470c09ab0881f00084f27775574739b0e4d8860c14c10e3f64ec953d79`
- Input `image`: 1×3×512×512 float RGB, 0–1. Input `mask`: 1×1×512×512, 1 where to fill.
- Output `output`: 1×3×512×512 float RGB, 0–255.

It was converted from Carve's ONNX export ([Carve/LaMa-ONNX](https://huggingface.co/Carve/LaMa-ONNX),
`lama_fp32.onnx`, SHA-256 `1faef5301d78db7dda502fe59966957ec4b79dd64e16f03ed96913c7a4eb68d6`) by
`scripts/remove-object-model/convert.py` in the Paint-A-Pint source. The script pins the batch size, simplifies
the graph, rewrites the FFT units' reversing slices as gathers, converts through PyTorch to a Core ML ML program,
checks it against onnxruntime (within 1 level of 255 on the test photos), and compiles it.

## Licence

LaMa's code and weights are released under the Apache License 2.0 by their authors, and so is this conversion
(see `LICENSE`). Copyright for the LaMa model belongs to its authors (Samsung AI Center).

Note: LaMa was trained on Places365, whose images are provided for non-commercial research and education.
