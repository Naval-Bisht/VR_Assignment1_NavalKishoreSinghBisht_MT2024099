

# 🌄 Panorama Stitching from Overlapping Images

## 📌 Introduction

This project stitches multiple overlapping images into a panoramic view using **OpenCV**, **NumPy**, and **Matplotlib**. Implemented in a Jupyter Notebook (`panorama_stitching.ipynb`), it explores three approaches to image stitching, ranging from manual feature-based methods to OpenCV’s automated solution. The input images are "P1.jpg", "P2.jpg", and "P3.jpg", stored in this folder, with the goal of creating seamless panoramas for applications like photography or mapping.

---

## 💁‍♂️ Folder Structure

```
Panorama_Stitching/
├── panorama_stitching.ipynb  # Jupyter Notebook with all models
├── P1.jpg  # First input image
├── P2.jpg  # Second input image
├── P3.jpg  # Third input image
├── model1_stitched_P12.jpg  # Model 1: P1 + P2 intermediate
├── model1_final_panorama.jpg  # Model 1: Final stitched result
├── model1_P1_keypoints.jpg  # Model 1: P1 keypoints
├── model1_P2_keypoints.jpg  # Model 1: P2 keypoints
├── model1_P12_keypoints.jpg  # Model 1: P12 keypoints
├── model1_P3_keypoints.jpg  # Model 1: P3 keypoints
├── model2_P1_to_P2_matches.jpg  # Model 2: P1-P2 matches
├── model2_P1_to_P3_matches.jpg  # Model 2: P1-P3 matches
├── model2_warped_P2.jpg  # Model 2: Warped P2
├── model2_warped_P3.jpg  # Model 2: Warped P3
├── model2_final_panorama.jpg  # Model 2: Final stitched result
├── model3_final_panorama.jpg  # Model 3: Final stitched result
├── README.md
```

---

## 🚀 Getting Started

### 🔹 1. Navigate to the Folder
Assuming the parent repository `VR_Assignment1_NavalKishoreSinghBisht_MT2024099` is cloned:
```bash
cd VR_Assignment1_NavalKishoreSinghBisht_MT2024099/Panorama_Stitching
```

### 🔹 2. Install Dependencies
Ensure Python and Jupyter are installed, then run:
```bash
pip install opencv-python numpy matplotlib jupyter
```

### 🔹 3. Prepare Input
The input images "P1.jpg", "P2.jpg", and "P3.jpg" are already in this folder.

### 🔹 4. Open the Jupyter Notebook
```bash
jupyter notebook panorama_stitching.ipynb
```
Run the cells corresponding to each model to generate panoramas.

### 🔹 5. View the Results
Each model produces:
- **Model 1**: Keypoint images, intermediate stitch, final panorama.
- **Model 2**: Feature match images, warped images, final panorama.
- **Model 3**: Final panorama only.
- Visualizations in Matplotlib and saved files in this folder.

---

## 🧪 Methods and Techniques

### 🎯 Model 1: Pairwise Stitching with SIFT
- **Steps**:
  1. Uses **SIFT** to detect keypoints and descriptors.
  2. Matches features with **Brute-Force Matcher** (KNN, 0.75 ratio test).
  3. Estimates homography with `cv2.findHomography` (RANSAC, 5.0 threshold).
  4. Warps images with `cv2.warpPerspective`.
  5. Stitches P1 and P2, then the result with P3, using simple overlay blending.
- **Outputs**:
  - "model1_stitched_P12.jpg": P1 + P2.
  - "model1_final_panorama.jpg": P12 + P3.
  - Keypoint visualizations (e.g., "model1_P1_keypoints.jpg").

### 🎯 Model 2: All-at-Once Stitching with SIFT
- **Steps**:
  1. Uses **SIFT** for keypoint detection and descriptors.
  2. Matches features with **Brute-Force Matcher** (KNN, 0.75 ratio test).
  3. Computes homography relative to P1 for P2 and P3.
  4. Warps all images into a wide canvas (3x width) with `cv2.warpPerspective`.
  5. Blends by overlaying non-zero pixels.
- **Outputs**:
  - "model2_final_panorama.jpg": Final panorama.
  - "model2_P1_to_P2_matches.jpg", "model2_warped_P2.jpg", etc.

### 🎯 Model 3: OpenCV Stitcher
- **Steps**:
  1. Uses OpenCV’s `cv2.Stitcher` class for automated stitching.
  2. Internally handles feature detection (likely SIFT/SURF), matching, homography, and blending.
  3. Processes all images (P1, P2, P3) in one step.
- **Outputs**:
  - "model3_final_panorama.jpg": Final panorama.

---

## ⚡ Optimizations
- **Canvas Size**: Model 1 uses 2x width, Model 2 uses 3x width to prevent cutoff.
- **Match Threshold**: Models 1 and 2 require >4 good matches to proceed, reducing failures.

---

## 📊 Results & Observations

| Feature               | Model 1 Outcome                          | Model 2 Outcome                          | Model 3 Outcome                          |
|-----------------------|------------------------------------------|------------------------------------------|------------------------------------------|
| **Feature Detection** | SIFT detects robust keypoints.           | SIFT detects robust keypoints.           | Internal (likely SIFT/SURF).             |
| **Matching Accuracy** | KNN with ratio test ensures good matches.| KNN with ratio test ensures good matches.| Automated, high accuracy.                |
| **Image Warping**     | Pairwise warping may accumulate errors.  | All-at-once warping aligns to P1.        | Seamless internal warping.               |
| **Blending Quality**  | Simple overlay may show seams.           | Overlay blending may show seams.         | Advanced blending (likely multi-band).   |

### 📌 Model Comparison Summary
- **Model 1**: Stepwise control, good for debugging with keypoints.
- **Model 2**: Efficient for multi-image stitching, aligns to one base.
- **Model 3**: Fully automated, best for simplicity and quality.

---

## 🔍 Challenges & Solutions

### ⚠️ Issues Faced
- **Insufficient Matches**: Not enough keypoints in low-overlap regions.
- **Perspective Distortion**: Warping can misalign distant objects.
- **Seams**: Simple overlay blending leaves visible edges.

### ✅ Solutions Implemented
- **Match Check**: Skips stitching if <4 matches (Models 1 and 2).
- **RANSAC**: Filters outliers in homography estimation (Models 1 and 2).
- **Canvas Adjustment**: Increases width to fit warped content.
- **Automation**: Model 3 uses OpenCV’s optimized blending.

---

## 📸 Sample Outputs

### 📌 Input Images
![P1](P1.jpg)
![P2](P2.jpg)
![P3](P3.jpg)

### 📌 Model 1: Keypoints and Panorama
![P1 Keypoints](model1_P1_keypoints.jpg)
![Final Panorama Model 1](model1_final_panorama.jpg)

### 📌 Model 2: Matches and Panorama
![P1-P2 Matches](model2_P1_to_P2_matches.jpg)
![Final Panorama Model 2](model2_final_panorama.jpg)

### 📌 Model 3: Final Panorama
![Final Panorama Model 3](model3_final_panorama.jpg)

---

## 🔑 Key Learnings & Takeaways
- **SIFT**: Reliable for detecting overlapping features.
- **RANSAC**: Improves homography accuracy by filtering outliers.
- **Pairwise vs. All-at-Once**: Model 1 offers control; Model 2 simplifies multi-image tasks.
- **Automation**: OpenCV Stitcher (Model 3) excels in ease and quality.
- **Visualization**: Keypoints and matches (Models 1 and 2) aid understanding.

This project demonstrates a progression from manual to automated panorama stitching techniques! 🚀
