# Lab 5 — Spatial Filtering: Smoothing and Sharpening

**ARTI 404 — Image Processing** · Session 5 · 100 minutes

**Outcome #2:** Write a program that implements fundamental image processing algorithms.

Everything for this lab lives in [`lab5.ipynb`](lab5.ipynb). The notebook is committed
with its outputs and figures embedded, so the results can be read without running it.

## Running it

The notebook needs `opencv-python`, `numpy`, `matplotlib` and `scikit-image`. Open
`lab5.ipynb`, select an interpreter that has them installed, then **Run All**.

## Images

The manual's unsharp masking step reads `images/Parrot.png`, which is not part of this
repository. **Step 0** uses `Parrot.png` if you put it in `labs/lab5/images/`; otherwise it
writes a stand-in and uses that. The assessment tasks use "an image of your choice", and
Step 0 saves those too, so the Lab 5 folder contains every image used:

| File | Source | Used in |
| --- | --- | --- |
| `images/coffee.png` | `skimage.data.coffee()` | Task 1, unsharp masking (stands in for `Parrot.png`) |
| `images/cameraman.png` | `skimage.data.camera()` | Assessment task 1, box filter |
| `images/astronaut.png` | `skimage.data.astronaut()` | Assessment task 2, Gaussian filters |
| `images/moon.png` | `skimage.data.moon()` | Assessment task 3, Laplacian sharpening |

## What the notebook does

### Procedural steps

1. **Task 1 — Unsharp masking:** blur with `cv2.GaussianBlur` (σ = 2), then
   `sharpened = image + 1.5 * (image - blurred)`, clipped to [0, 1]. Shows original,
   blurred and sharpened side by side, plus a zoomed crop of the same region in all three.

### Assessment

| Task | Implementation |
| --- | --- |
| 1. 7x7 box filter | `cv2.filter2D` with a `np.ones((7, 7)) / 49` kernel; checks the result equals `cv2.blur(img, (7, 7))`; original and smoothed side by side |
| 2. 5x5 and 21x21 Gaussian filters | `cv2.GaussianBlur` with σ derived from the kernel size (1.10 and 3.50); original, 5x5 and 21x21 side by side |
| 3. 3x3 Laplacian sharpening | Image to float32, `cv2.Laplacian(..., cv2.CV_32F, ksize=1)` (the standard `[[0,1,0],[1,-4,1],[0,1,0]]` kernel), then `np.clip(image_float - laplacian, 0, 1)`; plots original, Laplacian and sharpened |

## Resources

- <https://docs.opencv.org/4.x/d4/d13/tutorial_py_filtering.html>
- <https://datacarpentry.github.io/image-processing/06-blurring.html>
