# Enhancing Nighttime Road Images using Adaptive Histogram Equalization (CLAHE)

This project improves nighttime road images using **Contrast Limited Adaptive Histogram Equalization (CLAHE)** to boost visibility and safety. It measures brightness and contrast before and after enhancement and generates comparison results.

---

## Overview
Driving at night can be tough due to **low light and poor contrast**. This project uses **OpenCV's CLAHE** to enhance low-light images. This makes lane markings, road signs, and other objects clearer.

**Features:**
- Measures brightness and contrast before and after enhancement.
- Provides side-by-side comparison.
- Saves the enhanced image to a file.

---

## Requirements
Install the necessary dependencies:
```bash
pip install opencv-python numpy matplotlib
```
### Project Files
```
.
├── night_road.jpg          # Input image (your nighttime road photo)
├── comparison.png          # Comparison of original and enhanced (generated)
├── enhanced_road.jpg       # Enhanced output (generated)
├── enhance_night_road.py   # Main Python script
└── README.md               # Project documentation
```
### Code
```python
import os
import cv2
import numpy as np
import matplotlib
matplotlib.use('Agg')  
import matplotlib.pyplot as plt

# ---------- CONFIG ----------
IMAGE_PATH = "night_road.jpg"  # Change if needed
# ----------------------------

# Check if the image exists
if not os.path.exists(IMAGE_PATH):
    print(f"ERROR: Image file '{IMAGE_PATH}' not found!")
    exit()

# Load and convert to grayscale
image = cv2.imread(IMAGE_PATH)
gray = cv2.cvtColor(image, cv2.COLOR_BGR2GRAY)

# Function to calculate brightness and contrast
def get_stats(img):
    brightness = np.mean(img)
    contrast = img.std()
    return brightness, contrast

# Stats before enhancement
b_before, c_before = get_stats(gray)

# Apply CLAHE
clahe = cv2.createCLAHE(clipLimit=3.0, tileGridSize=(8, 8))
enhanced = clahe.apply(gray)

# Stats after enhancement
b_after, c_after = get_stats(enhanced)

# Create figure
fig, axes = plt.subplots(1, 2, figsize=(12, 6))

# Original image
axes[0].imshow(gray, cmap='gray')
axes[0].set_title(f"Original\nBrightness: {b_before:.1f}, Contrast: {c_before:.1f}")
axes[0].axis('off')

# Enhanced image
axes[1].imshow(enhanced, cmap='gray')
axes[1].set_title(f"Enhanced (CLAHE)\nBrightness: {b_after:.1f}, Contrast: {c_after:.1f}")
axes[1].axis('off')

plt.suptitle("Night Vision Enhancement for Road Safety", fontsize=16, color='blue')
plt.tight_layout()
plt.savefig("comparison.png")
print("Comparison image saved as comparison.png")

# Save enhanced image
cv2.imwrite("enhanced_road.jpg", enhanced)
print("Enhanced image saved as 'enhanced_road.jpg'")
```
### How to Run
Place your nighttime image as night_road.jpg in the project folder.

Run:
```bash
python enhance_night_road.py
```
### View results:
- enhanced_road.jpg → enhanced grayscale image.
- comparison.png → original vs enhanced side-by-side.

### Results
Before vs After Enhancement
| Original | Enhanced (CLAHE) |
|----------|------------------|

### Notes
Works best on low-light or nighttime images. Can be used for real-time enhancement with video streams. Prevents over-amplification of noise with clipLimit.

### License
Licensed under the MIT License; free to use and modify.

### Acknowledgements
Thanks to OpenCV for image processing tools, NumPy for numerical operations, and Matplotlib for visualizations.

---

If you also commit `night_road.jpg`, `enhanced_road.jpg`, and `comparison.png` to your GitHub repo, the **before/after table will display on the GitHub README page** without requiring users to run your code.

I can create a **more compact version** with just the essential code snippet and results if you want it to look cleaner.
