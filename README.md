# sEMG-Based Limb Movement Detection using CNN

Detect and classify limb movements from surface electromyography (sEMG) signals using a Convolutional Neural Network (CNN).

## Overview

This project implements a CNN-based system to identify different limb movements using sEMG signals. It includes preprocessing, training, and evaluation modules for both publicly available and custom datasets.

## Datasets Used

### 1. UCI sEMG for Basic Hand Movements

We use the [UCI sEMG for Basic Hand Movements dataset](https://archive.ics.uci.edu/dataset/313/semg+for+basic+hand+movements), which contains surface EMG recordings for 5 hand movements:

- Spherical
- Tip
- Palmar
- Lateral
- Cylindrical

**Details:**

- 5 healthy subjects (two males and three females) of the same age approximately (20 to 22-year-old) conducted the six grasps for 30 times each. The measured time is 6 sec. 
- 2-channel EMG
- Sampling rate: 500 Hz


### 2. Lab-Recorded 2-Channel sEMG Dataset

In addition to the UCI dataset, we also collected a custom sEMG dataset in our lab using two surface electrodes. This dataset captures:

- Basic hand gestures.
- 2-channel EMG data 
- Sampling via sEMG acquisition boards

This dataset serves as a lightweight alternative for testing on the model.

## Features

- Preprocessing (normalization, segmentation)
- Configurable CNN model for flexibility
- Support for multichannel and 2-channel data
- Evaluation with accuracy, confusion matrix, and plots

## Methodology

### Time-Series Segmentation

We process the continuous sEMG signal using a **sliding window segmentation** technique:

- The raw signal is divided into overlapping time segments ( 150 samples per segment).
- Each segment slides forward by a small step ( 15 samples), creating overlap.
- A label is assigned to each segment based on the most frequent class within that window.

This segmentation approach helps the model learn localized temporal patterns in muscle activity and increases the number of training samples, making it ideal for short gestures or limited datasets.

---

### CNN-Based Classification

We used a deep 1D Convolutional Neural Network (CNN) to classify the segmented sEMG signals.
After segmentation:

- Each segment ( 150 time steps × 2 channels) is used as input to a **1D Convolutional Neural Network (CNN)**.
- CNN layers automatically extract both temporal and inter-channel patterns from the sEMG data.
- This architecture efficiently captures both **short-term muscle activity patterns** and **channel-wise relationships**, 


The CNN outputs a probability distribution over possible movement classes. This architecture performs well for both the **multi-channel UCI dataset** and the **2-channel lab-recorded dataset**, showing good generalization.

### Architecture Summary:

- **Input Layer:** Reshapes the flattened segment into a 2D format (150, 2) to match the time-series structure.
- **First Convolution Block:**
  - Two consecutive 1D convolutional layers with 100 filters each and kernel size of 10
  - Activation: ReLU
- **Max Pooling:** Applies 1D max pooling with a pool size of 3 to reduce temporal resolution.
- **Second Convolution Block:**
  - Two additional 1D convolutional layers with 160 filters each and kernel size of 10
  - Activation: ReLU
- **Global Average Pooling:** Reduces each feature map to a single value, making the model robust to temporal shifts and variable-length sequences.
- **Dropout:** A dropout layer with a 0.5 rate to prevent overfitting.
- **Dense Output Layer:** A fully connected softmax layer with 6 output units (for 6 movement classes).

## 📈 Results

| Dataset           | Accuracy |
|------------------|----------|
| UCI Dataset       | **96%**  |        
| Lab-Recorded Data | **81%**  |  







