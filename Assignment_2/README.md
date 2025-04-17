# Augmented Reality with Planar Homographies

This repository implements augmented reality using planar homographies. The system detects a book cover in video frames, computes homography transformations, and projects virtual content onto the book cover.

## Overview

The implementation provides a comprehensive pipeline for creating augmented reality effects:
1. Feature detection and matching between a reference book cover and video frames
2. Robust homography estimation using RANSAC
3. Projecting virtual content onto detected book covers
4. Blending and rendering the augmented content

## Functions

### Image Processing

#### `convert_to_grayscale(image)`
- **Purpose**: Converts a color image to grayscale
- **Parameters**: `image` - Input color image in BGR format
- **Returns**: Grayscale version of the input image

#### `remove_black_bars(frame)`
- **Purpose**: Removes black borders from video frames
- **Parameters**: `frame` - Input video frame
- **Returns**: Cropped frame with black borders removed

### Video Handling

#### `load_first_frame(video_path)`
- **Purpose**: Extracts only the first frame from a video file
- **Parameters**: `video_path` - String containing the file path to the video
- **Returns**: First frame from the video

#### `load_video_frames(video_path)`
- **Purpose**: Loads all frames from a video file into memory
- **Parameters**: `video_path` - String containing the file path to the video
- **Returns**: List of all video frames

#### `save_ar_video(frames, output_path, fps=25)`
- **Purpose**: Saves processed frames as a video file
- **Parameters**:
  - `frames` - List of image frames to be saved as video
  - `output_path` - Path where the video file will be saved
  - `fps` - Frames per second (default: 25)

### Feature Detection and Matching

#### `get_corresponding_points(image1, image2)`
- **Purpose**: Identifies matching points between two images using SIFT features
- **Parameters**:
  - `image1` - First input image (grayscale)
  - `image2` - Second input image (grayscale)
- **Returns**: Keypoints and top 50 matches between images

### Homography Computation

#### `compute_homography_dlt(src_pts, dst_pts)`
- **Purpose**: Computes homography matrix using Direct Linear Transform (DLT) algorithm
- **Parameters**:
  - `src_pts` - Source points (coordinates in first image)
  - `dst_pts` - Destination points (corresponding coordinates in second image)
- **Returns**: 3x3 homography matrix

#### `ransac(src_pts, dst_pts, threshold=5.0, num_iterations=1000, early_stopping=True)`
- **Purpose**: Implements RANSAC algorithm for robust homography estimation
- **Parameters**:
  - `src_pts` - Source points (Nx2 array)
  - `dst_pts` - Destination points (Nx2 array)
  - `threshold` - Maximum pixel distance for inliers (default: 5.0)
  - `num_iterations` - Maximum number of RANSAC iterations (default: 1000)
  - `early_stopping` - Boolean to enable early termination (default: True)
- **Returns**: Best homography matrix and boolean mask of inliers

### AR Content Processing

#### `crop_ar_video(frame, book_corners)`
- **Purpose**: Crops and resizes a video frame to match book cover dimensions
- **Parameters**:
  - `frame` - Input video frame
  - `book_corners` - Coordinates of the four corners of the book cover
- **Returns**: Resized frame, book width, and book height

#### `create_ar_application()`
- **Purpose**: Creates an augmented reality application by processing each frame
- **Returns**: List of augmented video frames

## AR Pipeline

The complete augmented reality pipeline consists of:

1. **Feature Detection and Matching**: 
   - SIFT features are used to find robust correspondences between the book cover and video frames

2. **Homography Estimation**: 
   - RANSAC is used to compute transformation matrices while handling outliers

3. **Content Preparation**: 
   - AR content is cropped and resized to match book cover dimensions

4. **Warping and Blending**: 
   - AR content is warped to the detected book position in each video frame
   - Proper blending creates a seamless effect

5. **Video Output**: 
   - Final frames are combined into a video with the augmented reality overlay

## Usage

```python
# Load book cover and videos
cv_cover = convert_to_grayscale(cv2.imread('book_cover.jpg'))
book_frames = load_video_frames('book_video.mp4')
ar_source_frames = load_video_frames('ar_content.mp4')

# Create AR application
ar_frames = create_ar_application()

# Save output video
save_ar_video(ar_frames, 'output_ar_video.mp4', fps=25)
```

## Requirements

- OpenCV (cv2)
- NumPy