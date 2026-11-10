
# Edge Detection and Line Detection Using Computer Vision

A Python computer vision project exploring classical edge detection algorithms and line detection techniques.

The project compares **Sobel, Prewitt, Laplacian and Canny** edge detectors on HEp-2 fluorescence microscopy images, using quantitative evaluation and visual analysis. It also investigates the **Hough Transform** for identifying prominent straight lines in architectural images.

## Project Overview

The aim of this project was to understand how different image processing techniques identify boundaries and structural features, and how their performance changes depending on image characteristics.

The project is divided into three tasks:

1. Implement and evaluate Sobel, Prewitt and Laplacian edge detectors.
2. Apply Canny edge detection and compare its performance with classical methods.
3. Combine Canny edge detection with the Hough Transform to identify straight lines in an image.

## Technologies Used

- Python
- NumPy
- SciPy
- scikit-image
- Matplotlib
- Jupyter Notebook

## Task 1: Classical Edge Detection

### Methodology

Sobel, Prewitt and Laplacian operators were implemented manually using 3×3 convolution kernels and `scipy.signal.convolve2d`.

The HEp-2 microscopy images were preprocessed using:

- Grayscale conversion
- Intensity normalisation
- Image inversion for visualisation
- Gaussian smoothing to reduce noise
- Contrast Limited Adaptive Histogram Equalisation (CLAHE) to enhance local contrast

Different thresholds were tested to determine which settings produced edge maps that most closely matched the ground-truth images.

Performance was evaluated using:

- **Intersection over Union (IoU):** Measures overlap between detected and reference edges.
- **Dice coefficient:** Measures similarity between predicted and reference edge pixels.
- **Pixel accuracy:** Measures the proportion of correctly classified pixels.

### Results

| Image | Sobel IoU | Prewitt IoU | Laplacian IoU |
|---|---:|---:|---:|
| 43590 AM | 0.4068 | 0.4086 | 0.0827 |
| 10905 JL | 0.3250 | 0.3236 | 0.0412 |
| 9343 AM | 0.3548 | 0.3612 | 0.1068 |

**Key findings:**

- Prewitt achieved the highest IoU on two of the three images and generally produced smoother, thinner boundaries.
- Sobel successfully identified major cell outlines but generated thicker edges and more false positives.
- Laplacian performed poorly because its second-order derivative amplified noise and produced fragmented contours.

Overall, Prewitt provided the strongest quantitative performance across the three images.

## Task 2: Canny Edge Detection

### Methodology

The Canny edge detector was applied using `skimage.feature.canny`.

The implementation used the same preprocessing pipeline as Task 1 to support comparison between methods.

A grid search was performed over different low and high threshold combinations. Each resulting edge map was compared against the ground truth using IoU, and the highest-scoring threshold pair was selected.

### Results

| Image | Optimal Low Threshold | Optimal High Threshold | Canny IoU |
|---|---:|---:|---:|
| 43590 AM | 0.15 | 0.40 | 0.2421 |
| 10905 JL | 0.15 | 0.40 | 0.1454 |
| 9343 AM | 0.10 | 0.40 | 0.2055 |

**Key findings:**

- Canny produced thin, continuous and well-localised edges.
- Non-maximum suppression helped reduce thick and blurred contours.
- Double thresholding and hysteresis helped preserve connected boundaries while suppressing noise.
- Despite producing visually cleaner results, Canny achieved lower IoU scores than Sobel and Prewitt.

One explanation is that Canny generated thinner boundaries than those represented in the ground-truth images, reducing pixel-level overlap.

This demonstrated that visual quality and quantitative similarity do not always produce the same ranking of algorithms.

## Task 3: Hough Transform for Line Detection

### Methodology

The final task investigated straight-line detection in an architectural image featuring Old Joe, the University of Birmingham clock tower.

The process involved:

1. Converting the input image to grayscale.
2. Applying Canny edge detection.
3. Computing the straight-line Hough Transform using `skimage.transform.hough_line`.
4. Extracting prominent line candidates using `hough_line_peaks`.
5. Overlaying the detected lines onto the original image.

### Results

The Hough Transform successfully identified several prominent architectural structures, including:

- Vertical edges of Old Joe
- Building façades
- Strong horizontal and vertical features

However, the method also produced false positives in textured regions such as trees and grass.

Some weaker or partially obscured architectural lines were missed because they did not produce sufficiently strong or continuous edge responses.

**Conclusion:** The Hough Transform was effective for detecting dominant straight structures but was less reliable in regions containing complex textures or fragmented edges.

## Overall Findings

The experiments highlighted several important aspects of computer vision:

- **Preprocessing matters:** Gaussian smoothing and CLAHE helped improve edge visibility in noisy microscopy images.
- **Algorithm choice depends on the task:** Prewitt performed best quantitatively for the cell images, while Canny produced visually cleaner boundaries.
- **Evaluation metrics have limitations:** Thinner detected edges can achieve lower IoU despite appearing more accurate.
- **Line detection depends on edge quality:** The Hough Transform identified major structures but remained sensitive to noise and incomplete boundaries.

The project demonstrated the importance of combining quantitative evaluation with visual inspection when comparing image processing algorithms.

## Repository Structure

```text
Edge_Detectors/
├── task1_2_3.ipynb
├── Report.pdf
├── Data 30213/
│   ├── Bhamimage.jpeg
│   └── cells/
│       ├── 43590 AM.bmp
│       ├── 43590 AM Edges.bmp
│       ├── 10905 JL.bmp
│       ├── 10905 JL Edges.bmp
│       ├── 9343 AM.bmp
│       └── 9343 AM Edges.bmp
└── README.md
```

## Running the Project

### 1. Clone the repository

```bash
git clone https://github.com/nistha16/Edge_Detectors.git
cd Edge_Detectors
```

### 2. Install dependencies

```bash
pip install numpy scipy scikit-image matplotlib jupyter
```

### 3. Open the notebook

```bash
jupyter notebook task1_2_3.ipynb
```

Run the notebook cells in order to reproduce the edge detection experiments and visualisations.

The notebook uses relative file paths. Ensure the dataset directory and image filenames match the paths referenced in the code.

## Full Project Report

The complete report contains the methodology, threshold comparisons, experimental results, visual outputs and discussion of each algorithm.

**[Read the Full Computer Vision Report](./Report.pdf)**

## Project Status

Completed university computer vision coursework project.
