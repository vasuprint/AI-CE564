# Image Operations: sources and prepared examples

## Vintage paper

- Local file: `vintage_paper.png`, resized to the dimensions of `grace_hopper.jpg` and saved as PNG.
- Title: Free dark vintage paper page texture for layers (2982207584).
- Photographer: **D Sharon Pruitt / Pink Sherbet Photography**; photographed 28 October 2008. The description identifies the paper as being from an old family bible.
- Source: https://commons.wikimedia.org/wiki/File:Free_dark_vintage_paper_page_texture_for_layers_(2982207584).jpg
- Original image: https://upload.wikimedia.org/wikipedia/commons/1/1c/Free_dark_vintage_paper_page_texture_for_layers_%282982207584%29.jpg
- License: **CC BY 2.0**, https://creativecommons.org/licenses/by/2.0/
- Changes: downsampled from 2848×4272 to the portrait's dimensions with OpenCV `INTER_AREA`; the notebook further blends it with the portrait. Retain attribution when sharing the derived example.

## Simulated inputs

- `camera_added_object.png`: copy of `camera.png` with a filled rectangle, value 30, from `(340, 55)` to `(435, 120)` using OpenCV coordinates. This is a simulated change, not a second photograph. Camera source and rights: `ENHANCEMENT_SOURCES.md`.
- `grace_noisy_1.png` through `grace_noisy_4.png`: Grayscale versions of local `grace_hopper.jpg`, with separate Gaussian-noise samples from NumPy `default_rng(21)`, mean 0, standard deviation 25. Rounded, clipped to 0–255 and saved as uint8 PNG. Clipping can introduce bias near black and white; averaging is not guaranteed to restore the original exactly.
- The notebook loads these prepared examples directly to keep classroom code short. It neither overwrites the original images nor requires internet access.
- `grace_hopper.jpg` is the existing Matplotlib sample portrait of Rear Admiral Grace Hopper (U.S. Navy photograph), reused unchanged as the source portrait. The blend is a teaching effect, not an authentic historical photograph.
