# ACV-Project-Pose-Estimation

# Human Pose Estimation in Crowded Scenes



This repository presents our efforts to improve **human pose estimation** in crowded images. We noticed that standard models often perform poorly in such scenarios and explored a series of methods to address these limitations, combining classical computer vision techniques with patch-based inference strategies.

---

## Table of Contents

- [Motivation](#motivation)  
- [Initial Approaches](#initial-approaches)  
- [Keypoint-Based Adaptive Patching Method](#keypoint-based-adaptive-patching-method)  
- [Results](#results)  
- [Observed Limitations](#observed-limitations)  
- [Future Work](#future-work)  
- [References](#references)  

---

## Motivation

Standard **human pose estimation models** struggle in crowded scenes where multiple humans overlap or are close to each other. Common issues include:

- Merging nearby humans into a single pose  
- Failing to detect all people  
- Poor accuracy for diverse clothing styles (e.g., long garments, hijabs, niqabs, chadors)  

Our goal was to improve pose estimation under these challenging conditions.

---

## Initial Approaches

We first attempted classical methods to segment the image and reduce crowding effects:

1. **Human Detection via HOG + SVM or Template Matching**  
   - Detect humans and crop the image into smaller patches  
   - Each patch contains a few people, allowing more focused pose inference  
   - **Problems:** Limited detection accuracy; many humans were missed  

2. **Edge Detection + Blob Detection (MSER)**  
   - Attempted to generate patches based on image edges or blobs to avoid cutting through humans  
   - **Problems:**  
     - Edge detection (Canny) produced messy outputs in crowded areas  
     - MSER failed to detect full human bodies  

---

## Keypoint-Based Adaptive Patching Method

To overcome previous limitations, we designed a **density-aware patching strategy**:

1. **Keypoint Detection**  
   - Applied **Harris corner detection** to extract keypoints representing areas of high activity  
   - ![Harris Keypoints](path/to/keypoints_image.png)

2. **Adaptive Patching**  
   - Split the image into variable-sized patches based on keypoint density  
     - Denser regions → smaller patches  
   - Applied edge detection to merge small patches that intersected many edges (likely cutting through humans)  
   - ![Adaptive Patches](path/to/patches_image.png)

3. **Patch-Based Pose Inference**  
   - Performed pose estimation on each patch individually  
   - Merged results to form the full image pose map  
   - ![Final Pose Estimation](path/to/final_output_image.png)

**Advantages:**  
- Improved detection in crowded regions  
- Reduced merging of close humans compared to original model  
- Better coverage of individual human poses  

---

## Results

- The patch-based approach significantly improves pose estimation in crowded scenes  
- Allows the model to focus on smaller groups of people without losing context  
- Reduces errors caused by overlapping individuals  

Example images at each stage:

| Stage | Image |
|-------|-------|
| Keypoints | ![Keypoints](path/to/keypoints_image.png) |
| Patches | ![Patches](path/to/patches_image.png) |
| Edge Merging | ![Edges](path/to/edges_image.png) |
| Final Output | ![Final Pose](path/to/final_output_image.png) |

---

## Observed Limitations

Even with improved patch-based inference, some challenges remain:

1. **Cultural Clothing Variations**  
   - Loose garments, hijab, niqab, or chador can lead to missed humans or merged poses  
   - Hand and foot keypoints are often inaccurately estimated  
   - Example: ![Flaw in Hijab Detection](path/to/hijab_flaw_image.png)

2. **Remaining Accuracy Gaps**  
   - Our classical preprocessing methods may still miss humans in extremely dense crowds  

---

## Future Work

- Investigate **learning-based detection** to replace classical patching methods for more robust results  
- Extend evaluation across diverse cultural and clothing scenarios  
- Explore **crowd-specific pose estimation models** or **multi-scale inference strategies**  
- Improve integration to handle extremely dense crowds without patch-based preprocessing  

---

## References

- Cao, Zhe, et al. "Realtime Multi-Person 2D Pose Estimation Using Part Affinity Fields." *CVPR*, 2017.  
- Harris, Chris, and Mike Stephens. "A Combined Corner and Edge Detector." *Alvey Vision Conference*, 1988.  
- Dalal, Navneet, and Bill Triggs. "Histograms of Oriented Gradients for Human Detection." *CVPR*, 2005.  

---

## License

This project is licensed under the MIT License.  

---

*Note:* All images in this README are illustrative; replace `path/to/...` with your actual image file paths.
