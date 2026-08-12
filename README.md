#  MRI Tumor Detection using Image Processing

A MATLAB-based image processing project for detecting and identifying possible tumor regions in brain MRI images using **grayscale conversion, brain masking, binary segmentation, connected-component analysis, and region-based morphological features**.

This project was developed as part of an **Image Processing / Medical Imaging assignment**.

> **Note:** This is an educational image-processing project and is **not intended for clinical diagnosis or medical decision-making**.

---

##  Overview

The objective of this project is to analyze brain MRI images and determine whether a possible tumor region is present.

Instead of using a deep learning model, this implementation uses **classical digital image processing techniques** and geometric properties of connected regions.

The detected regions are evaluated using:

* Area
* Circularity
* Eccentricity
* Solidity

If a connected component satisfies the predefined conditions, it is classified as a potential tumor region.

---

##  Methodology

The complete processing pipeline is:

```text
Input MRI
    ↓
Grayscale Conversion
    ↓
Brain Masking
    ↓
Noise Removal
    ↓
Tumor Candidate Segmentation
    ↓
Connected Component Analysis
    ↓
Region Property Extraction
    ↓
Rule-Based Classification
    ↓
Tumor Localization & Visualization
```

---

##  Processing Steps

### 1. Input MRI Image

The MRI image is loaded using MATLAB's `imread()` function.

If the input image is RGB, it is converted into grayscale using `rgb2gray()`.

```matlab
I = imread('T_img7.jpg');

if ndims(I) == 3
    I = rgb2gray(I);
end
```

Grayscale images are used because the subsequent segmentation process is based on pixel intensity.

---

### 2. Brain Masking

A brain mask is generated to remove unwanted regions such as the skull and background.

Pixels with intensity greater than 20 are initially selected.

Small connected components are removed using `bwareaopen()`, followed by hole filling using `imfill()`.

```matlab
mask = I > 20;
mask = bwareaopen(mask,500);
mask = imfill(mask,"holes");
```

This step helps restrict subsequent tumor analysis to the brain region.

---

### 3. Tumor Candidate Segmentation

Dark regions within the brain mask are extracted using an intensity threshold.

```matlab
bw = (I < 60) & mask;
bw = bwareaopen(bw,100);
```

The thresholded image is then cleaned by removing small regions that are considered noise.

---

### 4. Connected Component Analysis

Connected components are identified using:

```matlab
cc = bwconncomp(bw);
```

Region properties are then calculated using `regionprops()`.

The following properties are extracted:

```matlab
stats = regionprops(cc,...
    'Area',...
    'Circularity',...
    'Eccentricity',...
    'Solidity',...
    'Centroid');
```

These properties describe the size, shape, and geometry of each segmented region.

---

##  Tumor Detection Criteria

Each connected component is evaluated using predefined thresholds.

A region is considered a potential tumor when:

| Feature      |                 Condition |
| ------------ | ------------------------: |
| Area         | > 3000 and < 20000 pixels |
| Circularity  |                    > 0.35 |
| Eccentricity |                    < 0.91 |
| Solidity     |                    > 0.25 |

The classification rule implemented in MATLAB is:

```matlab
if stats(k).Area > 3000 && stats(k).Area < 20000 && ...
   stats(k).Circularity > 0.35 && ...
   stats(k).Eccentricity < 0.91 && ...
   stats(k).Solidity > 0.25
```

When all conditions are satisfied, the region is marked as a potential tumor.

---

##  Output

The program calculates and displays:

* Brain area in pixels
* Detected tumor area in pixels
* Number of detected tumor regions
* Classification result
* Tumor region visualization
* Connected component visualization

Example output format:

```text
MRI ANALYSIS

Brain Area = 217914.00 pixels
Tumor Area = 17935.00 pixels
Tumor Count = 2
Classification = Tumor
```

For a normal image, the program can produce:

```text
MRI ANALYSIS

Brain Area = 140463.00 pixels
Tumor Area = 0.00 pixels
Tumor Count = 0
Classification = Normal
```

## The assignment results demonstrate a tumor case with **2 detected regions** and a normal case with **0 detected regions**.

##  Visualization

The program generates several visualizations:

### Processing stages

1. Original MRI
2. Brain mask
3. Tumor candidate binary image
4. Tumor region

### Final visualization

Detected tumor regions are highlighted using red boundaries.

The program also displays individual connected components with their corresponding component numbers.

---

## 🛠️ Technologies Used

* **MATLAB**
* Image Processing Toolbox
* Digital Image Processing
* Binary Image Segmentation
* Connected Component Analysis
* Morphological Image Processing
* Region Property Analysis

### MATLAB Functions Used

```text
imread()
rgb2gray()
bwareaopen()
imfill()
bwconncomp()
regionprops()
imshow()
visboundaries()
fprintf()
```

---

##  Project Structure

```text
MRI-Tumor-Detection/
│
├── README.md
├── tumor_detection.m
│
├── images/
│   ├── tumor/
│   │   └── ...
│   │
│   └── normal/
│       └── ...
│
└── results/
    └── ...
```

> Replace the filenames/folder structure above with your actual GitHub structure if it differs.

---

##  How to Run

### Requirements

* MATLAB
* Image Processing Toolbox

### Steps

1. Clone/download this repository.

2. Open the project folder in MATLAB.

3. Place the MRI image in the project directory.

4. Change the filename in the MATLAB script:

```matlab
I = imread('T_img7.jpg');
```

5. Run the MATLAB script.

6. The command window will display the MRI analysis results and MATLAB will generate the corresponding visualizations.

---

##  Features

* Automatic grayscale conversion
* Brain region extraction
* Skull/background suppression
* Binary segmentation
* Noise removal
* Connected component analysis
* Region-based feature extraction
* Rule-based tumor detection
* Tumor area estimation
* Tumor count estimation
* Tumor boundary visualization

---

##  Detection Approach

The project uses a **rule-based segmentation and classification approach**.

Rather than training a machine learning or deep learning model, the algorithm determines whether a region resembles the expected tumor characteristics according to predefined geometric thresholds.

This makes the project relatively simple, interpretable, and suitable for understanding the fundamentals of **medical image segmentation and feature-based analysis**.

---

##  Limitations

This implementation has several limitations:

* It uses fixed intensity thresholds.
* Detection depends strongly on MRI image characteristics.
* The area and shape thresholds are manually selected.
* Different MRI modalities or acquisition settings may require different parameters.
* The method may produce false positives or false negatives.
* It has not been clinically validated.
* It is not a machine-learning or deep-learning diagnostic model.

Therefore, the output should be considered **experimental/educational rather than a medical diagnosis**.

---

##  Future Improvements

Possible improvements include:

* Automatic threshold selection
* Adaptive segmentation
* Better skull stripping
* Image normalization
* Contrast enhancement
* Morphological refinement
* Extraction of additional texture features
* Evaluation on a larger MRI dataset
* Quantitative performance evaluation
* Machine learning-based classification
* CNN/Deep Learning-based tumor segmentation
* Integration with medical imaging platforms such as 3D Slicer

---

##  Author

**Suraj Hazarika**

M.Tech — Medical Imaging and Informatics
Indian Institute of Technology Kharagpur

---

##  Academic Context

This project was developed as part of coursework in **Medical Imaging / Image Processing** and demonstrates the application of fundamental digital image processing techniques to MRI analysis.
