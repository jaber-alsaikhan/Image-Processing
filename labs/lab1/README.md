# Lab 1 – Basics of Programming with Python

**Course:** ARTI403 – Image Processing
**Session outcome:** Explain how digital images are represented and manipulated in a computer.

## Overview

This lab walks through loading, visualizing, storing, and manipulating a digital
image as a NumPy array, using three different Python imaging libraries side by
side: **OpenCV**, **Pillow (PIL)**, and **scikit-image (skimage)**.

Instead of depending on an external file (e.g. `images/cameraman.tif`), the
notebook uses skimage's built-in sample image `ski.data.camera()` so it runs
standalone with no extra downloads.

## Files

| File | Description |
|---|---|
| `lab1.ipynb` | Jupyter notebook with all lab tasks |

Running the notebook also creates:
- `images/cameraman.png` – the working image written to disk
- `new_image_cv2.jpg`, `new_image_pil.jpg`, `new_image_ski.jpg` – the same image re-saved via each library

## Requirements

- Python 3.x
- `opencv-python`
- `numpy`
- `scikit-image`
- `pillow`
- `matplotlib`

Install with:

```bash
pip install opencv-python numpy scikit-image pillow matplotlib
```

(If this project uses the `.venv` in the repo root, activate it first —
`.venv\Scripts\activate` on Windows — then run the command above.)

## Tasks

1. **Load and visualize an image** — read the same image using OpenCV
   (`cv2.imread`), PIL (`Image.open`), and skimage (`ski.io.imread`), and
   display all three alongside `ski.data.camera()` in a 2×2 subplot grid.
2. **Store an image** — save the loaded image back to disk using OpenCV
   (`cv2.imwrite`), PIL (`.save`), and skimage (`ski.io.imsave`).
3. **Display an image as an array** — print each loaded image's shape and
   raw NumPy array values.
4. **Assessment: array operations with NumPy** — crop, horizontally flip,
   brighten, and threshold the image array, then visualize the results.

## How to Run

Open `lab1.ipynb` in Jupyter, VS Code, or Spyder and run all cells top to
bottom. Two figure windows will appear: one comparing the three loading
methods, and one showing the NumPy array operations from the assessment.
