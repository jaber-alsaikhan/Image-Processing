# Lab 2 — Digital Image Fundamentals

**ARTI 403 — Image Processing** · Session 2 · 100 minutes

**Outcome #1:** Explain how digital images are represented and manipulated in a computer.

Everything for this lab lives in [`lab2.ipynb`](lab2.ipynb). The notebook is committed
with its outputs and figures embedded, so the results can be read without running it.

## Running it

The notebook needs `opencv-python`, `numpy`, `matplotlib`, `pillow` and `scikit-image`.
They are already installed in the repository's `.venv`, along with `ipykernel` so the
notebook can run against it.

Open `lab2.ipynb` and select the `.venv` interpreter, then **Run All**. In VS Code the
interpreter is pinned by `.vscode/settings.json`; if it is not picked up, use
`Ctrl+Shift+P` → *Python: Select Interpreter* → `.\.venv\Scripts\python.exe`.

> Running it with a different Python will fail at `import cv2`, because the packages are
> installed in `.venv` and not globally.

## Images

The manual loads `lena_gray_256.tif`, `cameraman.tif`, `A.png` and `B.png` from
`../images`. Those files are not part of this repository, so **Step 0 generates
equivalents** and writes them to `labs/images/` before anything else runs:

| Generated file | Source | Stands in for |
| --- | --- | --- |
| `portrait_gray_256.png` | `skimage.data.astronaut()`, grayscale, 256×256 | `lena_gray_256.tif` |
| `cameraman_256.png` | `skimage.data.camera()`, 256×256 | `cameraman.tif` |
| `A.png` | filled disk, binary | `A.png` |
| `B.png` | filled square overlapping the disk, binary | `B.png` |

Every later step then opens them by path exactly the way the manual does. These files are
in `.gitignore` — re-running the notebook recreates all of them, so they are not tracked.

## What the notebook does

### Procedural steps

1. **Image Sampling and Quantization** — `sample_image()` downsamples by an integer
   factor with nearest-neighbour interpolation; `quantize_image()` reduces the number of
   gray levels; `plot_images()` shows original, sampled and quantized side by side. Run at
   a sampling factor of 14 and 9 gray levels, as in the manual.
2. **Arithmetic Operations** — adds two 400×400 images. Shown twice: raw `uint8`, where
   the sum wraps around (200 + 100 → 44), and clipped to 0–255, which is normally what is
   wanted.
3. **Sets and Logical Operations** — union of the two binary shapes with `|`, plus the
   intersection (`&`), symmetric difference (`^`) and complement (`~`).

### Task 1 — change the sampling and quantization parameters

Sampling factors 1 → 32 and gray levels 256 → 2, each swept in one figure so the two
effects can be compared directly.

- **Sampling** controls *spatial* resolution. Each doubling of the factor quarters the
  pixel count (256×256 → 8×8 at factor 32). Fine detail goes first — badge lettering, the
  rocket in the background — and the image turns blocky.
- **Quantization** controls *intensity* resolution. Spatial detail is untouched, but
  smooth gradients break into flat bands. This false contouring is clearly visible from
  about 16 levels down, and at 2 levels the image is pure black and white.

### Task 2 — operations on two gray-scale images

Both images are read into arrays, converted to `int16` for the arithmetic so nothing
overflows before it is clipped, and combined pixel by pixel:

| Operation | Implementation |
| --- | --- |
| Subtraction | `clip(A − B, 0, 255)` |
| Add a constant of 175 | `clip(A + 175, 0, 255)` |
| Set difference `A ∩ Bᶜ` | `minimum(A, 255 − B)` |
| Symmetric difference | `maximum(A − B, B − A)` |
| Intersection | `minimum(A, B)` |
| Union *(for reference)* | `maximum(A, B)` |

The set operations use the standard gray-scale definitions, where the complement is
`255 − A`, union is the pixelwise maximum and intersection the pixelwise minimum.

## Notes

- Display goes through matplotlib rather than PIL's `Image.show()`, so the notebook runs
  start to finish without opening external image viewers.
- Paths resolve from the notebook's own directory, falling back to the working directory
  when `__file__` is undefined — which is the case inside a notebook kernel.

## Reference

OpenCV documentation — <https://docs.opencv.org/>
