# Lab 4 — Intensity Transformations and Filtering (Spatial Domain)

**ARTI 404 — Image Processing** · Session 4 · 100 minutes

**Outcome #2:** Write a program that implements fundamental image processing algorithms.

Everything for this lab lives in [`lab4.ipynb`](lab4.ipynb). The notebook is committed
with its outputs and figures embedded, so the results can be read without running it.

## Running it

The notebook needs `opencv-python`, `numpy`, `matplotlib` and `scikit-image`. Open
`lab4.ipynb`, select an interpreter that has them installed, then **Run All**.

## Images

The manual's thresholding step reads `../images/Parrot.png`, which is not part of this
repository. **Step 0** uses `Parrot.png` if you put it in `labs/images/`; otherwise it writes
a stand-in and uses that. Step 0 also saves the skimage sample images the lab uses, so they
are in `labs/images/` alongside the earlier labs' images:

| File | Source | Used in |
| --- | --- | --- |
| `coffee.png` | `skimage.data.coffee()` | Task 1, thresholding (stands in for `Parrot.png`) |
| `moon.png` | `skimage.data.moon()` | Task 2, assessment tasks 1–2 |
| `rocket.png` | `skimage.data.rocket()` | Assessment task 3 (reference) |
| `chelsea.png` | `skimage.data.chelsea()` | Assessment task 3 (source) |

## What the notebook does

### Procedural steps

1. **Task 1 — Thresholding** with `cv2.threshold(..., cv2.THRESH_BINARY)` at 0, 50, 100,
   150 and 200, shown in one grid next to the original. Display goes through matplotlib
   instead of `cv2.imshow`/`cv2.waitKey`.
2. **Task 2 — Histogram processing** on the moon image: contrast stretching between the
   2nd and 98th percentiles with `exposure.rescale_intensity`, each image shown above its
   256-bin histogram.

### Assessment

| Task | Implementation |
| --- | --- |
| 1. Stretch between the 3rd and 80th percentiles | `np.percentile` + `exposure.rescale_intensity`; also reports how many pixels get clipped to 255 (~21%) |
| 2. Histogram equalization | `exposure.equalize_hist`, scaled back to 0–255; shown with its histogram, plus an original-vs-equalized comparison with the cumulative distribution overlaid |
| 3. Histogram matching | `exposure.match_histograms(chelsea, rocket, channel_axis=-1)`, with per-channel histograms and CDFs for source, reference and matched |

## Resources

- Hands-On Image Processing with Python (book)
- <https://scikit-image.org/docs/stable/auto_examples/color_exposure/plot_equalize.html>
- <https://scikit-image.org/docs/0.24.x/auto_examples/color_exposure/plot_histogram_matching.html>
