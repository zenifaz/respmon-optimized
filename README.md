# RespMon-Optimized: Real-Time Contactless Respiratory Monitor

An optimized, real-time computer vision application designed to monitor human breathing patterns (Breaths-Per-Minute) entirely through non-contact video analysis. By tracking subtle pixel-intensity shifts and chest displacement, this system serves as a non-invasive alternative to traditional biological contact sensors.

## 🛠️ Key Modifications & Bug Fixes
This repository refactors and stabilizes the legacy baseline `respmon` implementation, addressing critical runtime blocks, hardware handling, and platform compatibility issues:

* **Orientation Alignment:** Fixed internal frame rotation matrices to support standard upright, vertical camera feeds, preventing tracking window mismatches.
* **Non-Blocking UI Loop:** Replaced unstable asynchronous quit handlers with a native, millisecond-precision keyboard intercept loop, preventing application freezes and ensuring clean webcam hardware releases upon exit (`ESC` / `q`).
* **Robust Video Exporting:** Overhauled a failing single-channel grayscale recording pipeline into an OS-compliant, 3-channel BGR mapped architecture to ensure output `.mp4` video captures are universally playable.
* **Dynamic Data Inputs:** Expanded input parser flexibility to accept both live hardware webcam indexes (`0`) and standard pre-recorded benchmark video files (`.mp4`) for repeatable algorithm testing.

---

## 📈 The Core Pipeline

The system bridges computer vision and digital signal processing through a multi-stage execution framework:
1.  **Calibration Phase:** Captures an initial 128-frame buffer to analyze the background and isolate rhythmic frequency domains.
2.  **Temporal Differencing:** Processes sequential frame subtractions ($Frame_t - Frame_{t-1}$) to eliminate static environmental features.
3.  **Spatial Variance & Thresholding:** Highlights high-activity movement fields, discarding minor sensor noise via a binary mask.
4.  **ROI Localization:** Isolates the precise geometric boundaries of the chest contour to establish a bounded Region of Interest (ROI).
5.  **Signal Conversion:** Translates continuous pixel-intensity changes within the ROI into a live, real-time graph, deriving exact Breaths-Per-Minute (BPM) from wave peak frequencies.

---

## 🚀 Getting Started

### Prerequisites
Ensure you have Python 3.8+ installed along with the required graphic and math processing dependencies:
```bash
pip install opencv-python numpy pyqtgraph PyQt5