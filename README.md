# Vision-Based Pick and Place with Multi-Region Planning 🤖📦

This project implements a **vision-based pick-and-place system** for a large workspace, focusing on **efficient perception, region-aware planning, and object sorting**.  
The system is designed to handle **multiple object configurations and reachability constraints** using a combination of **deep learning, point cloud processing, and trajectory planning**.

---

## 📌 Problem Overview

The pick-and-place environment is **large and complex**, making it impractical to rely on a single camera view or full-scene image stitching.

### Key Challenge
- Large workspace
- Objects located in multiple regions with different constraints
- Orientation variability
- Reachability limitations

### Core Idea
Instead of continuously moving the camera, the system uses **predefined lookup positions** and **region-specific planning strategies**.

---

## 👁️ Perception Strategy

- **Total Lookup Positions:** 7  
- Each lookup position captures a subset of the workspace
- Object detection is performed independently at each position

### Perception Model
- **Model:** YOLOv4  
- **Training:** Custom dataset  
- **Confidence Threshold:** 85%  
- **Environment:** MATLAB  

The perception subsystem outputs:
- Bounding boxes
- Class indices (`classIndex`)
- Detection confidence

---

## 🗺️ Workspace Decomposition

The workspace is divided into **five distinct regions**, each with its own planning logic:

1. **Static Region**
2. **Orientation Change Region**
3. **Box Region (Objects inside container)**
4. **Object Change Region**
5. **Out-of-Reach Region**

---

## 🧠 Planning & Control Pipeline

### 1️⃣ Position Estimation
- Bounding box coordinates from YOLOv4
- Pixel coordinates are **deprojected** to obtain `PointStamped` data
- Camera subsystem provides 3D object location

---

### 2️⃣ Pick and Place Strategy (Region-wise)

#### 🔹 Static Region
- Fixed object orientations
- Hardcoded pick and place poses
- Sorting based purely on `classIndex`

---

#### 🔹 Orientation Change Region
- Object orientation estimated using **Principal Component Analysis (PCA)**
- Pipeline:
  1. Extract point cloud from YOLO bounding box
  2. Compute centroid
  3. Apply PCA to determine dominant object direction
  4. Align gripper accordingly

---

#### 🔹 Box Region
- Same logic as orientation change region
- PCA-based orientation estimation
- Robust to cluttered object placement

---

#### 🔹 Object Change Region
- Dynamic object configurations
- Same PCA + IK-based planning approach

---

#### 🔹 Out-of-Reach Region
- Direct straight-line motion is not feasible
- Solution:
  - Generate **2–3 cubic spline trajectories**
  - Introduce intermediate checkpoints
  - Ensure splines remain **above ground surface**
  - Plan trajectory segment-by-segment

This avoids unstable or physically invalid spline generation.

---

## 🦾 Motion Planning

- **Inverse Kinematics (IK)** used for all pick and place motions
- Trajectories generated based on:
  - Region constraints
  - Object orientation
  - Reachability

---

## 🗂️ Object Sorting Logic

- Sorting is based on the **detected object class (`classIndex`)**
- Each class maps to a predefined placement location
- Consistent sorting across all regions

---

## 🧪 Key Techniques Used

- YOLOv4 (custom-trained)
- Point cloud processing
- PCA for orientation estimation
- Inverse Kinematics
- Cubic spline trajectory generation
- Region-aware planning

---

## 🎥 Demo & Files

All related files, scripts, and demonstration videos can be found here:

🔗 **Google Drive:**  
https://drive.google.com/drive/folders/1K72kiB2UANAzJ-EH_QYFgYtcLRqNQV9N

---

## ⚠️ Notes & Limitations

- Performance depends on detection accuracy
- PCA assumes sufficient point cloud density
- Hardcoded poses are region-specific
- Requires careful calibration between camera and robot

---

## 👤 Author

**Shakthi Bala**  
Robotic Manipulation | Vision-Based Planning | Motion Planning | Perception Systems
