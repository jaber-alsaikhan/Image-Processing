# Lab 7 — Laplacian Filter, and Mathematical & Statistical Filters

**ARTI 404 — Image Processing** · Session 7

The lab came as two notebooks, **7(A) Laplacian Filter** and **7(B) Maths & Statistics
Filters**. Both parts are in [`lab7.ipynb`](lab7.ipynb), with a PDF export in
[`lab7.pdf`](lab7.pdf). The notebook is committed with its outputs and figures embedded,
so the results can be read without running it.

## Running it

The notebook needs `opencv-python`, `numpy`, `matplotlib` and `scikit-image`. Open
`lab7.ipynb`, select an interpreter that has them installed, then **Run All**. The
random seed is fixed, so the noisy images and the PSNR table are the same every run.

## Images

| File | Used in |
| --- | --- |
| `images/moon.jpg` | Part A, Laplacian sharpening (the manual's `moon.jpg`) |
| `images/noise.jpg` | Part B, the clean square-and-circle image that noise is added to (the manual's `noise.jpg`) |
| `images/salt_pepper.jpg` | Part B, a real salt-and-pepper photo for the median filter |

## What the notebook does

### Part A — Laplacian filter

1. **Manual kernel:** applies the 4-neighbour kernel `[[0,1,0],[1,-4,1],[0,1,0]]` with
   `cv2.filter2D(..., cv2.CV_64F)`, then `convertScaleAbs` and `img + |Laplacian|` as in
   the manual.
2. **Fixing the sharpening:** shows that `img + laplacian_abs` overflows `uint8` (541
   pixels wrap around and turn black) and loses the sign of the edge. Sharpens correctly
   with `g = f - ∇²f` in float, for both the 4-neighbour and the 8-neighbour kernel.
3. **`cv2.Laplacian`:** blurs with a 3x3 Gaussian first to reduce noise, then applies
   `cv2.Laplacian`. Checks that it equals `filter2D` with the 4-neighbour kernel, and
   compares sharpening with and without the blur.

### Part B — Mathematical and statistical filters

1. **Adding noise:** Gaussian, salt & pepper, exponential and uniform noise with the
   manual's `add_noise`, plus salt-only and pepper-only images to show which filter removes
   which impulse.
2. **Mean filters:** arithmetic (`cv2.blur` and a manual kernel, which match exactly, at
   3x3/5x5/7x7), geometric, harmonic and contra-harmonic (`Q = ±1.5`, each on the noise
   it is meant for and with the wrong sign).
3. **Order-statistic filters:** median (on salt & pepper noise and on the real photo, 3x3
   and 5x5), max/min (`cv2.dilate` / `cv2.erode`) and midpoint.
4. **Comparison:** every filter on every noise type, scored with PSNR against the clean
   image:

| Noise | Best filter (3x3) | PSNR noisy → filtered |
| --- | --- | --- |
| Gaussian | Arithmetic mean | 20.9 → 30.0 dB |
| Salt & pepper | Median | 18.5 → 50.9 dB |
| Exponential | Min | 25.7 → 33.5 dB |
| Uniform | Midpoint | 26.9 → 37.2 dB |

### Fixes to the manual's code

- `img + laplacian_abs` wraps around in `uint8`; the sharpened images are computed in
  float, subtract the signed Laplacian, and are clipped to [0, 255].
- The arithmetic mean's Method 2 filtered the clean `img`; it now filters `noise_gauss`.
- The median filter was applied to `noise_gauss` but titled "Salt & Pepper"; it is now
  applied to `noise_sp`.
- `np.random.randint(0, i - 1, ...)` never hit the last row/column (the upper bound is
  already exclusive); it is `randint(0, i, ...)`.
- The contra-harmonic, max and min filters are shown on the impulse noise they target
  instead of only on Gaussian noise.

## Resources

- <https://docs.opencv.org/4.x/d4/d13/tutorial_py_filtering.html>
- <https://docs.opencv.org/4.x/d5/d0f/tutorial_py_gradients.html>
