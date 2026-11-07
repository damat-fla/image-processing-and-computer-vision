# Image Processing and Computer Vision – University Project

This repository contains the solutions to two assignments developed by **Flavio D'Amato and Alessio La Torre** as part of the *Image Processing and Computer Vision (IPCV)* course at the University of Bologna.

The project is divided into two independent assignments, each addressing a different computer vision task through traditional image processing techniques and deep learning approaches.

## Assignment 1 – Product Recognition of Books

### Task

The goal of this assignment is to develop a computer vision system capable of recognizing books on a shelf, given a reference image for each book.

For each book appearing in a scene, the system must identify all its occurrences and provide:
- The number of detected instances.
- The bounding box coordinates of each instance.
- The area of each bounding box.
- A visual representation of the detections overlaid on the original scene.

The assignment requires the use of **traditional computer vision techniques**, without deep learning.

### Implemented Solution

Our approach combines feature matching and template matching to detect multiple instances of the same book.

The pipeline consists of two main stages:

**1. First Instance Detection**

- SIFT is used to extract keypoints and descriptors from the reference image and the shelf scene.
- Brute-force descriptor matching is performed using Lowe's ratio test.
- RANSAC is used to estimate the homography between the reference image and the scene.
- The homography is then used to project the reference image corners onto the scene, obtaining the first bounding box.

**2. Subsequent Instance Detection**

- The first detected book is rectified using perspective transformation and used as a template.
- Template matching based on Normalized Sum of Squared Differences (SSD) is performed to locate additional occurrences.
- Small variations in scale and rotation are considered to improve robustness.
- A hysteresis thresholding strategy is adopted to classify candidate matches.
- Ambiguous matches are further validated through SIFT feature matching.
- Previously detected regions are masked to avoid duplicate detections.

The complete pipeline is tested on all available model-scene combinations in the provided dataset.

## Assignment 2 – Pet Classification

### Task

The goal of this assignment is to develop an image classification system capable of recognizing **37 different breeds of cats and dogs** using the Oxford-IIIT Pet dataset.

The assignment is divided into two parts:

- **Part 1:** Design and train a custom convolutional neural network (CNN) from scratch, including an ablation study to evaluate the contribution of its architectural components.
- **Part 2:** Fine-tune a pretrained ResNet-18 model and optimize its training hyperparameters to improve classification performance.

### Implemented Solution

#### Part 1 – Custom CNN

We designed a custom CNN based on a modular architecture called `TripleConvBlock`.

Each block consists of:
- Three convolutional layers.
- Batch Normalization.
- LeakyReLU activation.
- Max Pooling for spatial downsampling.
- Optional skip connections.

The complete architecture stacks four convolutional blocks, followed by Global Average Pooling and a fully connected classification layer.

We also conducted an **ablation study** to evaluate the impact of different architectural and training choices, including skip connections, activation functions, Batch Normalization, network depth, channel width, Global Average Pooling, learning rate scheduling and data augmentation.

**Test Accuracy: 72.54%**

#### Part 2 – Transfer Learning with ResNet-18

We used the ResNet-18 architecture pretrained on ImageNet-1K (V1), replacing its original classification layer to accommodate the 37 target classes.

The fine-tuning process was divided into two experiments:

**2A – Initial Fine-Tuning**

The pretrained ResNet-18 was trained using the same hyperparameters adopted for the custom CNN. This experiment provided a baseline for evaluating the benefits of transfer learning.

**Test Accuracy: 79.47%**

**2B – Optimized Fine-Tuning**

To improve performance, we adopted a two-stage training strategy:

1. **Classifier training:** The pretrained feature extractor is frozen, and only the final classification layer is trained for 20 epochs.
2. **Full network fine-tuning:** The feature extractor is unfrozen, and the entire network is fine-tuned for an additional 30 epochs using a significantly lower learning rate.

We also increased the input image resolution to preserve more visual details during classification.

**Test Accuracy: 91.28%**

### Results Summary

| Model | Test Accuracy |
|---|---:|
| Custom CNN (Part 1) | 72.54% |
| ResNet-18 (Part 2A) | 79.47% |
| ResNet-18 (Part 2B) | **91.28%** |

The optimized ResNet-18 achieves the best performance.

## Technologies

The project was developed using Python and the following libraries:

- **OpenCV:** Traditional computer vision algorithms and image processing.
- **PyTorch:** Neural network implementation and training.
- **Torchvision:** Pretrained ResNet-18 and image transformations.
- **NumPy:** Numerical computations.
- **Matplotlib:** Visualization of images, detections and experimental results.

## Dataset

The assignments use two different datasets:

- **Assignment 1:** Reference book images (*Models*) and shelf images (*Scenes*) provided for the course.
- **Assignment 2:** [Oxford-IIIT Pet Dataset](https://www.robots.ox.ac.uk/~vgg/data/pets/), containing images of 37 cat and dog breeds, using the train-validation-test split provided for the course.


## Authors

- Flavio D'Amato
- Alessio La Torre

**Course:** Image Processing and Computer Vision (IPCV)  
**University:** University of Bologna