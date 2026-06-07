# Dual-Engine 3D Reconstruction & Ground-Truth Metric Calibration Pipeline

## 🚀 Project Overview
This repository documents an advanced photogrammetry pipeline utilizing both **VisualSFM** and **COLMAP** core engines to generate a high-fidelity 3D structural reconstruction of an outdoor campus environment. Because Structure-from-Motion (SfM) algorithms inherently yield scale-invariant outputs, a key objective was executing an absolute **similarity transformation** using real-world ground-truth telemetry to calibrate the final point cloud to a true **1:1 metric scale**.

---

## 🛠 Engineering Toolset
* **Sparse Reconstruction (SfM Core):** VisualSFM & COLMAP
* **Dense Point Cloud Generation (MVS):** COLMAP (PatchMatch Stereo)
* **Spatial Calibration & Analysis:** CloudCompare
* **Environment Design & Visualization:** Blender

---

## 📐 The Technical Pipeline

### 1. Dual-Engine Feature Tracking & Sparse Alignment
To evaluate reconstruction resilience, feature extraction and tracking were executed across two separate algorithmic frameworks:
* **VisualSFM:** Leveraged for rapid initial SIFT feature matching and spatial camera path generation.
* **COLMAP:** Utilized to perform highly rigorous bundle adjustment, successfully generating precise camera intrinsic/extrinsic matrices and establishing a dense camera frustum network.

### 2. Multi-View Stereo Dense Surface Mapping
Following sparse orientation validation, the COLMAP Multi-View Stereo (MVS) workspace was deployed. PatchMatch stereo matching was applied across the aligned image sets to convert sparse tie points into a multi-million-point dense geometric asset (`fused.ply`).

### 3. Ground-Truth Metric Transformation
Because the initial model operated purely within arbitrary engine units, a manual similarity transformation was applied using fixed architectural constraints:
* **Ground Truth Constraint ($D_{real}$):** An external structural window width was field-measured at **1.4224 meters** (56 inches).
* **Cloud Baseline Distance ($D_{cloud}$):** Point-picking inspection within CloudCompare yielded an unscaled baseline of **0.392998 units**.
* **Global Scale Factor Calculation:** $$s = \frac{D_{real}}{D_{cloud}} = \frac{1.4224}{0.392998} \approx \mathbf{3.6193}$$

* **Execution:** An isotropic scale factor of `3.619` was globally applied via CloudCompare, successfully mapping the entire 3D space directly into true real-world meters.

---

## 📊 Quality Control & Diagnostics
* **Mean Re-projection Error:** 0.7 pixels — confirming high mathematical stability during bundle adjustment. 
* **Scale Integrity:** Post-transformation point-picking validation on control structures yielded an absolute deviation error of <1%, making the asset fully compatible with real-world BIM and engineering software.
