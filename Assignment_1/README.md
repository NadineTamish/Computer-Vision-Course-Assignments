
# Part One:
# Image Cartoonifier

## Overview
This project applies image processing filters to convert real-world images into cartoon-style images. The approach involves enhancing edges and smoothing flat regions to achieve a cartoon-like effect.

## Process Steps

### 1. Converting Image to RGB Format
Before applying filters, the input image is converted to RGB format to ensure color consistency.

### 2. Generating a Black-and-White Sketch
To create a sketch effect, we use an edge-detection filter. Various edge detection techniques exist, including Sobel, Scharr, Laplacian, and Canny-edge detectors. For this project, we use the Laplacian filter since it produces edges that closely resemble hand-drawn sketches.

#### Steps:
- Convert the image to grayscale.
- Apply a **median filter** for noise reduction.
- Use the **Laplacian edge detector** to highlight edges.
- Apply a **binary threshold** to create a black-and-white sketch.

### 3. Generating a Color Painting
A **bilateral filter** is applied to smooth flat areas while preserving edges, creating a painting-like effect. Instead of using a single large filter, multiple small bilateral filters are applied for efficiency.

#### Bilateral Filter Parameters:
- **Color Strength**: Controls how much color detail is preserved.
- **Positional Strength**: Determines how far the filter spreads.
- **Size**: Defines the filter kernel size.
- **Repetition Count**: Controls the intensity of the smoothing effect.

### 4. Creating the Cartoon Effect
To finalize the cartoon effect:
- Overlay the **black-and-white sketch** onto the **color painting**.
- Preserve only the non-edge pixels from the painting.

## Implementation Details
- Image filtering is performed using OpenCV.
- The Laplacian filter detects edges effectively.
- Bilateral filtering enhances color regions while keeping edges sharp.
- The final cartoon effect is achieved by combining the edge-detected sketch with the smoothed color image.

## Usage
1. Load an RGB image.
2. Apply the median filter for noise reduction.
3. Use the Laplacian filter to detect edges.
4. Apply a binary threshold to obtain a sketch.
5. Use a bilateral filter to smooth the image.
6. Combine the sketch and painting for the final cartoon effect.

