# Lab 6 — Frequency Domain

**ARTI 404 — Image Processing** · Session 6 · 100 minutes

**Outcome #1:** Write a program that implements fundamental image processing algorithms.

Everything for this lab lives in [`lab6.ipynb`](lab6.ipynb), with a PDF export in
[`lab6.pdf`](lab6.pdf). The notebook is committed with its outputs and figures embedded,
so the results can be read without running it.

## Running it

The notebook needs `numpy`, `scipy`, `matplotlib` and `scikit-image`. Open `lab6.ipynb`,
select an interpreter that has them installed, then **Run All**.

## Images

Both tasks use the Cameraman image. **Step 0** saves it from skimage's sample data into
`images/` and the notebook reads it from there, so the Lab 6 folder contains the image used:

| File | Source | Used in |
| --- | --- | --- |
| `images/cameraman.png` | `skimage.data.camera()` | Task 1 and the assessment task (resized to 256x256) |

## What the notebook does

### Procedural steps

1. **Task 1 — Filtering in the frequency domain:** builds the Laplacian frequency response
   `H(u, v) = -4π²(u² + v²)` on an `np.fft.fftfreq` grid, multiplies it with `fft2` of the
   image and takes `ifft2` back. Shows the original, the Laplacian spectrum and the spatial
   Laplacian output (the manual's code), then the same spectra centred with `fftshift`, and
   sharpens the image with the single frequency-domain filter `1 - c * H`.

### Assessment

| Task | Implementation |
| --- | --- |
| 1. Sobel filters in the frequency domain | Embeds `sobel_x` and `sobel_y` at the centre of an image-sized zero array, `ifftshift`s the centre to (0, 0) and takes `fft2` (the manual's `center_embed_kernel`). Multiplies with the image FFT and inverts to get `Gx`, `Gy` and the gradient magnitude. Also plots the filters' frequency responses, and checks the result against spatial `ndimage.convolve(..., mode="wrap")` (they match to ~1e-15) |

## Resources

- <https://numpy.org/doc/stable/reference/routines.fft.html>
- <https://docs.opencv.org/4.x/de/dbc/tutorial_py_fourier_transform.html>
