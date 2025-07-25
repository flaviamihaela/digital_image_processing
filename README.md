# PCB Hole Fault Detection Using Perspective Transformation and Morphological Filtering

This project explores an image processing pipeline to detect drilling faults in Printed Circuit Boards (PCBs) by aligning a reference overlay and applying morphological operations. The method relies on manual homography, grayscale thresholding, and blob analysis to identify missing or misplaced PCB holes from bitmap images.

## Overview
Given a standard PCB design and a scanned or photographed board under test, this project performs:

- Manual perspective correction via homography based on 4 selected reference holes

- Morphological filtering using dilation, erosion, and blob analysis

- Detection of hole defects (missing or misaligned holes)

The original implementation was built using Simulink with .slx models [![Open in MATLAB Online](https://www.mathworks.com/images/responsive/global/open-in-matlab-online.svg)](https://matlab.mathworks.com/open/github/v1?repo=flaviamihaela/digital_image_processing)

## Methodology
1. Homography-Based Registration
- Assumes all boards have consistent planar geometry except for hole faults.

- Uses a generated reference image with circular markers at known hole positions.

- Selects 4+ corresponding hole centers in both the reference and actual image.

- Defines top-left, top-right, bottom-right, bottom-left pairs for projective transform.

- The Warp block applies a transformation matrix to align the test PCB to the reference overlay.

Final image resolution: 750×750 pixels;
Scale: 0.0987 mm/pixel (for a 74mm² board);

2. Thresholding
- Grayscale histogram reveals a dip around bin 38/256 → normalized threshold: 0.15

- Converts the image to binary with a relational threshold operator.

3. Morphological Operations
- Dilation structuring element (5×6 square): removes noise-sized elements

- Erosion structuring element (2×2): removes table texture artifacts

4. Blob Analysis
- Filters based on area: minimum area - 0 pixels; maximum area - 80 pixels (to discard oversized holes)

- Reports blob centroid positions, area, and count

## Conclusions
- Some false positives: 2 holes incorrectly flagged as missing

- Manual alignment is highly sensitive to 1–2 pixel misplacements

- No compensation for lighting variation or multiplicative noise

## Future Work
- Automate hole detection using edge/corner detection (e.g. Harris or Hough)

- Improve preprocessing using histogram equalization and homomorphic filtering

- Explore adaptive thresholding and lighting-invariant feature extraction

## Dependencies
- MATLAB + Simulink (Image Processing Toolbox, Video Viewer, Blob Analysis)

## Running the Pipeline
1. Overlay Marker Generation
- Create a reference image with known hole positions.

2. Coordinate Collection

- Use Video Viewer → Image Editor to get (x, y) of 4 holes on both images

- Enter them into the Homography matrix generator

3. Image Warping

- Use Warp block to align images to reference frame

4. Thresholding & Morphology

- Apply 0.15 threshold

- Dilation → Erosion

- Use Blob Analysis to find hole regions




