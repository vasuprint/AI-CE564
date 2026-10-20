# Spatial Filtering: figure sources

## Original teaching diagrams

- `kernel_sliding_steps.png`: steps 1, 2 and 4 of a 3 x 3 mean kernel sliding over a 5 x 5 input.
- `kernel_sliding.gif`: all nine valid positions, stride 1, no padding.
- Both diagrams were generated specifically for Week 9 using Matplotlib and Pillow. They are not reproduced textbook figures.
- Input: the values 10, 20, ..., 250 in row-major order. The valid output is `[[70, 80, 90], [120, 130, 140], [170, 180, 190]]`.
- Orange marks the current patch/output; cyan marks the input patch center. All reads use the original input.
- Copies are embedded as notebook Markdown attachments for offline viewing. GIF playback depends on the notebook renderer; a static step diagram is also provided.

## References

- OpenCV, Smoothing Images: https://docs.opencv.org/4.x/d4/d13/tutorial_py_filtering.html
- OpenCV, filter2D API (including correlation, anchor, border and output depth): https://docs.opencv.org/4.x/d4/d86/group__imgproc__filter.html#ga27c049795ce870216ddfb366086b5a04
- Dive into Deep Learning, Section 7.2, Convolutions for Images: https://d2l.ai/chapter_convolutional-neural-networks/conv-layer.html

These sources were used as conceptual references; no external figure was downloaded or embedded.
Camera is reused from the local sample data; see `ENHANCEMENT_SOURCES.md` for its source and CC0 attribution.
Gaussian noise and step-edge inputs are simulated in notebook code, not additional photographs.

## Current Week 9 teaching notebook

- `wee9_real.ipynb` uses the local `grace_hopper.jpg` in Grayscale for Mean, Sharpen, Sobel X, and random-kernel demonstrations. Image provenance is recorded in `ENHANCEMENT_SOURCES.md`.
- Gaussian noise is simulated with standard deviation 20 and NumPy seed 9, then clipped to the display range. Random weights use a normal distribution with mean 0, standard deviation 0.5, and seed 12; they are not learned weights.
- The first random kernel is reused as the first of four independently applied kernels. Their signed response maps share a symmetric color scale, with no activation function, bias, or training.
- All full-image examples use OpenCV cross-correlation with replicated borders and Float outputs. The small manual sliding example and embedded animation use stride 1 without padding.
