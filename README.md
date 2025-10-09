# Cryo-EM-Ultrastructures
## Overview

This repository hosts an AI-assisted image segmentation and analysis pipeline for _Pantoea sp. YR343_ cryo-electron microscopy (cryoEM) datasets.The workflow automates membrane thickness measurements, flagella detection, and field-of-view (FOV) screening from low-dose, high-resolution cryoEM micrographs eliminating the need for slow manual annotation.By integrating deep-learning based segmentation (YOLOv11) with quantitative post-processing, this toolkit provides a scalable and reproducible way to study bacterial morphology under hydrated, near-native conditions.

<img width="600" height="175" alt="image" src="https://github.com/user-attachments/assets/e3e8fb0f-e450-49b4-a50b-d10e14670436" />




# Tools and Key Features
Our segmentation models enable high-througput identification and quantification of various bactetrial ultrastructures.
## 1. Automated Membrane Segmentation
Eanbles cell envelope(Outer-Inner membrane) thickness calculation accurately.
Uses YOLOv11 for instance segmentation of inner and outer membranes.
Eanbles high-throughput generation of per-cell thickness maps and statistical summaries (mean, area variance, eccentricity) for multiple bacterial images
## 2. Flagella Detection and Quantification
Detects and skeletonizes flagella filaments to compute:
a) Flagella length (curvilinear and tip-to-tip).
b) Degree of coiling (flagellar curvature ratio).
c) Nearest-distances between flagella and bacterial envelopes.
d) Enables automated proximity and orientation analysis for multiple bacteria per field of view.
## 3. Field-of-View (FOV) Screening
Identifies and counts bacteria in low-magnification images, enabling rapid pre-screening.
Facilitates dataset-level mapping of bacterial distribution.
Processes large cryoEM datasets in batches with consistent quality metrics.
Output includes overlay images, CSV summaries, and QC plots for downstream statistical analysis which are automatically saved into the output directory.

# Run the Tool  
Each module in this repository is designed to run interactively in Google Colab.Users can open the corresponding .ipynb notebook for any tool, connect to a Colab runtime, and execute the code cell by cell.Below are the steps: 
1. Open the desired notebook from the notebooks/ folder (e.g.,Membrane_Thickness_Tool.ipynb, Flagella_Length_Tool.ipynb, Interaction_Network_Tool.ipynb).
2. Click “Open in Colab” or upload it to your own Colab workspace.
3. Mount Drive / set paths and run cells sequentially to reproduce the full workflow. Steps include model loading, predicting on a test image (4096*4096) image.  
Each notebook is self-contained and can be executed independently; no prior installation on your local machine is required.

# Training on a Custom Dataset
All datasets must be pre-processed by first normalizing the raw images using percentile-based adaptive normalization followed by Roboflow preprocessing pipeline.The exported datasets will be ready for YOLO model development and training through Ultralytics framework.

## **Preprocessing in Roboflow**
RobloFlow contains modules for data loading, annotation, spliting and data augmentation. Follow the below preprocessing steps before exporting your dataset:

1) Upload cryoEM images (png) to a new Roboflow project.
2) Draw polygon masks for each class:
   0 → Outer Membrane, 1 → Inner Membrane (Thickness model).
   0 → Flagella, 1 → bacteria (optional, for flagella detection model)
   0 → Bacteria in the FOV (optional, for FOV model) 
3) Selct and apply augmentations and preprocessing options suitable for your dataset
4) Resize to 640×640 px (YOLO standard input size). Or enter re-sizing numbers suitable for the daatset. 
5) Export the dataset in a format compatible with user's chosen YOLO model — for example, YOLOv11 TXT + YAML format (used in this workflow) or COCO Segmentation format if training with other PyTorch implementations.Ultralytics framework natively supports both structures for model training and validation.
## **Model building**
Organize the exported data as Train/Valid/Test dataset. Upload the dataset folders, set the paths in the yaml file and run the YOLOv11 pre-trained segmentation models from Ultralytics platform.
