# imfilter Workbench

A single-file, interactive visualizer for 2‑D spatial filtering (`imfilter`-style correlation and convolution). Type in your own image matrix and kernel, then step through the sliding window one output pixel at a time to see exactly how each value is computed.

No build step, no dependencies — it's one HTML file you can open directly in a browser or host anywhere (e.g. GitHub Pages).

## Features

- **Editable input matrix** — set rows/cols (up to 8×8) and type any values directly into the grid
- **Editable kernel** — set rows/cols (up to 7×7), type values directly, or load a preset:
  - `[-1 0 1]` edge kernel
  - Sobel horizontal / vertical (3×3)
  - Laplacian (3×3)
  - Box blur (3×3)
  - Identity (3×3)
- **Side-by-side kernel view** — compare the kernel *as entered* against the kernel *as applied*, with an arrow showing when/whether it's flipped 180° for convolution
- **Full option set**:

  | Option | Description |
  |---|---|
  | `'corr'` | Filtering by correlation (default) |
  | `'conv'` | Filtering by convolution (kernel rotated 180° before sliding) |
  | `P` | Boundary extended by padding with a constant value `P` (editable, default 0) |
  | `replicate` | Boundary extended by replicating the outer border values |
  | `symmetric` | Boundary extended by mirror-reflecting across the border |
  | `circular` | Boundary extended by treating the image as one period of a 2‑D periodic function |
  | `'same'` | Output is the same size as the input (default) |
  | `'full'` | Output is the same size as the extended (padded) image |

- **Step-through transport** — step forward/back, auto-play at adjustable speed, reset, or click any output cell to jump straight to its computation
- **Live formula strip** — shows the exact multiply-accumulate expression for the current output pixel
- **Diverging output coloring** — positive values in green, negative in red, so results are readable at a glance
- **Dark / light theme toggle** — defaults to dark

## Usage

1. Download `imfilter_visualizer.html`
2. Open it in any modern browser (double-click, or `open imfilter_visualizer.html`)
3. Edit the matrix and kernel, choose your filtering mode / boundary option / output size, and press **▶ play** or step through manually

To host it (e.g. GitHub Pages), just commit the HTML file and enable Pages on the repo — it's fully static.

## How it works

The engine pads the input matrix according to the chosen boundary mode, optionally flips the kernel 180° (for convolution), then slides it across the padded matrix computing a weighted sum at each position:

```
out(r, c) = Σ Σ  kernel(i, j) · padded(r + i, c + j)
```

- **`'same'`** padding uses `floor(kernel_size / 2)` on each side
- **`'full'`** padding uses `kernel_size - 1` on each side
- Even-sized kernels will show a warning, since `'same'` padding becomes asymmetric in that case (the classic textbook treatment assumes odd kernel dimensions)


